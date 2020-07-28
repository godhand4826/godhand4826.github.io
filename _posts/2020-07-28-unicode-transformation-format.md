---
layout: post
title:  "UTF (Unicode Transformation Format)"
date:   2020-7-28 16:05:03 +0800
categories: utf
---
## BOM
- Byte Order Mark
- 位於檔案最前面(相當於檔案的header)
- 用來暗示這串 `bytes` 是使用 `utf` 編碼
- 用來區別 `big-endian` 與 `little-endian`

## utf8
- 每個字符佔據 `1~4 bytes`, 可表示所有字符
- 每個字符的第 `frist byte` 可以得知此字符總共佔據多少個 `byte`
    - `1 byte ` 字符 (U+00000 ~ U+00007F): 0xxxxxxx (其實就是`ascii`)
    - `2 bytes` 字符 (U+00080 ~ U+0007FF): 110xxxxx 10xxxxxx
    - `3 bytes` 字符 (U+00800 ~ U+00FFFF): 1110xxxx 10xxxxxx 10xxxxxx
    - `4 bytes` 字符 (U+10000 ~ U+10FFFF): 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
- `utf8` 的 `BOM` 為 `0xEF 0xBB 0xBF`
- `utf8` 解碼時, 一次只解讀 `1 byte`, 所以並不會受 `Endianness` 所影響, 所以不需 `BOM`, 就可以正確解讀
- `ascii` 無須而外處理即可當作 `utf8` 使用, 而 `utf8` 只有在字符皆為 `ascii` 內, 且檔首無 `BOM`, 才可以轉成`ascii`
- 計算字串總長度, 或是取第N個字符, 因為不等長的特性所以都需從頭走訪一遍

## uft16
- 每個字符佔據 `2 bytes` 或 `4 bytes`, 可表示所有字符
    - `2 bytes` 字符 (U+000000 ~ U+00D7FF, U+00E000 ~ U+00FFFF): xxxxxxxx xxxxxxxx
    - `4 bytes` 字符 (U+010000 ~ U+10FFFF): 110110xx xxxxxxxx 110111xx xxxxxxxx
- `utf16le` 的 `BOM` 為 `0xFF 0xFE`
- `utf16be` 的 `BOM` 為 `0xFE 0xFF`
- `utf16` 解讀時, 即使用錯 `Endianness` 也都會找到對應的字符, 所以必須有 `BOM`, 才能確保正確解讀
- 計算字串總長度, 或是取第N個字符, 因為不等長的特性所以都需從頭走訪一遍

## uft32
- 每個字符都固定佔據 `4 bytes`, 可表示所有字符
- `utf32le` 的 `BOM` 為 `0xFF 0xFE 0x00 0x00`
- `utf32be` 的 `BOM` 為 `0x00 0x00 0xFE 0xFF`
- `utf32` 解讀時, 即使用錯 `Endianness` 也都會找到對應的字符, 所以必須有 `BOM`, 才能確保正確解讀

## CLI
- 使用 `printf` 加入 `BOM`
    ```bash
    $ (printf '\xEF\xBB\xBF'; $UTF8) > $UTF8_BOM
    $ (printf '\xFF\xFE'; cat $UTF16LE) > $UTF16LE_BOM
    $ (printf '\xFE\xFF'; cat $UTF16BE) > $UTF16BE_BOM
    $ (printf '\xFF\xFE\x00\x00'; cat $UTF32LE) > $UTF32LE_BOM
    $ (printf '\x00\x00\xFE\xFF'; cat $UTF32BE) > $UTF32BE_BOM
    ```
- 使用 `dd` 移除 `BOM`
    ```bash
    $ dd bs=3 skip=1 if=$UTF8_BOM of=$UTF8 &>/dev/null
    $ dd bs=2 skip=1 if=$UTF16LE_BOM of=$UTF16LE &>/dev/null
    $ dd bs=4 skip=1 if=$UTF32BE_BOM of=$UTF32BE &>/dev/null
    ```
- 使用 `dd` 轉換 `Endianness`
    ```bash
    $ dd conv=swab if=$UTF16LE of=$UTF16BE &>/dev/null
    $ dd conv=swab if=$UTF16LE_BOM of=$UTF16BE_BOM &>/dev/null
    ```
- 使用 `iconv` 轉換編碼
    - 在使用 `iconv` 在 `utf`編碼間轉換編碼時需注意
        - 以 `utf16` 跟 `utf32` 作為輸出格式時, 會自動幫你在輸出加上 `BOM`
        - 以 `utf16` 跟 `utf32` 作為輸入格式時, 會自動幫你去掉輸入的 `BOM`
    ```bash
    $ iconv -f utf8 -t utf16 $UTF8 > $UTF16LE_BOM
    $ iconv -f utf8 -t utf16 $UTF8_BOM > $UTF16LE_BOM_BOM
    $ iconv -f utf8 -t utf16le $UTF8 > $UTF16LE
    $ iconv -f utf8 -t utf16le $UTF8_BOM > $UTF16LE_BOM
    $ iconv -f utf16 -t utf8 $UTF16LE_BOM > $UTF8
    $ iconv -f utf16le -t utf8 $UTF16LE_BOM > $UTF8_BOM
    ```
