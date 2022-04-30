---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: CURL
  - java: JAVA
  - plaintext: RAW
  - python: PYTHON
  - php: PHP
  - csharp: C#

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the الفبا API
---

# الفبا

نویسه‌خوان الفبا تصویر اسناد فارسی، انگلیسی و عربی را می‌خواند و در قالب متن مرتب ارائه می‌دهد.

برای دسترسی به واسط برنامه‌نویس الفبا نیاز به یک TOKEN_KEY معتبر دارید که برای احراز هویت استفاده می‌شود. لطفا برای آزمایش سامانه، این متغیر را در تقاضاهای نمونه، جای‌گذاری کنید. سوال هم اگر دارید، لطفا برای آدرس alefba@roshan-ai.ir بنویسید.

# خواندن سند

این تابع، یک سند را دریافت می‌کند و متن صفحات آن را در قالب JSON باز می‌گرداند. خروجی نویسه‌خوان شامل بخش‌های نوشته (پاراگراف)، جدول و تصویر است. مکان و ابعاد هر بخش در خروجی مشخص شده است و اطلاعات کامل خطوط متن در بخش نوشته ظاهر می‌شود. برای هر خط متن، ویژگی احتمال صحت هم قرار داده شده که نشان می‌دهد ابزار نویسه‌خوان چقدر از نتیجه تحلیل، مطمئن است.


## نمونه ارسال با لینک سند

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
} \
      https://alefba.roshan-ai.ir/api/read_document/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/read_document/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/read_document/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/read_document/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/read_document/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": true,
    "priority": 3
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
{
  "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
  "pages": [{
    "page_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1",
    "width": 2125,
    "height": 2750,
    "text": "بوته\n\nدرس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
    "parts": [
      {
        "type": "text",
        "direction": "rtl",
        "box": "209 305 1711 449",
        "text": "درس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
        "lines": [
          {
            "probability": 1.0,
            "box": "211 305 1707 57",
            "text": "درس‌های دانشگاهی معمولا با پروژه‌هایی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها"
          },
          ...
        ]
      },
      ...
    ]
  },{
    "page_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=2",
    "width": 2125,
    "height": 2750,
    "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین\n...",
    "parts": [
      {
        "type": "text",
        "direction": "rtl",
        "box": "210 110 1714 294",
        "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین\n...",
        "lines": [
          {
            "probability": 1.0,
            "box": "210 110 1706 58",
            "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین"
          },
          ...
        ],
      },
      ...
    ]
  }]
}

```

`POST /api/read_document/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
<dl>
<strong>type</strong>
<br>
<br>
Value: 
<span className="enum-container">
<span>
general
</span>
<span style="font-family:VazirCode;">
سندهای عمومی
</span>
<span>
ID-card
</span>
<span style="font-family:VazirCode;">
کارت‌های شناسایی
</span>
<span>
excel
</span>
<span style="font-family:VazirCode;">
جدولهای مالی
</span>

</span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی نوع سند را مشخص می‌کند</p>
<br><br>
<dl>
<strong>fix_orientation</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی، الفبا سعی می‌کند چرخش ۹۰، ۱۸۰ و یا ۲۷۰ درجه تصویر را اصلاح کند</p>
<br><br>
<dl>
<strong>word_positions</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی اطلاعات مکانی واژه‌ها در خروجی قرار می‌گیرد</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر این ویژگی فعال باشد، کاربر منتظر می‌ماند تا نتیجه تحلیل آماده شود؛ در غیر این صورت، تقاضای تحلیل دریافت می‌شود و کاربر با استفاده از واسط «وضعیت سند» از میزان پیشرفت تحلیل مطلع می‌شود. به این ترتیب پس از پایان پردازش، تقاضای جدیدی برای پردازش سند ارسال می‌شود و این بار تقاضا با نتیجه مناسب پاسخ داده می‌شود.</p>
<br><br>
<dl>
<strong>priority</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    3
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی اولویت درخواست را تعیین می‌کند. مقدار آن می‌تواند بین ۱ تا ۴ باشد. عدد کمتر نشان‌دهندهٔ اولویت بالاتر است.</p>
<br><br>
## نمونه درخواست به صورت ناهمگام

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
} \
      https://alefba.roshan-ai.ir/api/read_document/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/read_document/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/read_document/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/read_document/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/read_document/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "fix_orientation": true,
    "word_positions": false,
    "type": "general",
    "wait": false,
    "priority": 3
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
  {
    "state":"PENDING",
    "task_ids": "..."
  }


```

`POST /api/read_document/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
<dl>
<strong>type</strong>
<br>
<br>
Value: 
<span className="enum-container">
<span>
general
</span>
<span style="font-family:VazirCode;">
سندهای عمومی
</span>
<span>
ID-card
</span>
<span style="font-family:VazirCode;">
کارت‌های شناسایی
</span>
<span>
excel
</span>
<span style="font-family:VazirCode;">
جدولهای مالی
</span>

</span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی نوع سند را مشخص می‌کند</p>
<br><br>
<dl>
<strong>fix_orientation</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی، الفبا سعی می‌کند چرخش ۹۰، ۱۸۰ و یا ۲۷۰ درجه تصویر را اصلاح کند</p>
<br><br>
<dl>
<strong>word_positions</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی اطلاعات مکانی واژه‌ها در خروجی قرار می‌گیرد</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر این ویژگی فعال باشد، کاربر منتظر می‌ماند تا نتیجه تحلیل آماده شود؛ در غیر این صورت، تقاضای تحلیل دریافت می‌شود و کاربر با استفاده از واسط «وضعیت سند» از میزان پیشرفت تحلیل مطلع می‌شود. به این ترتیب پس از پایان پردازش، تقاضای جدیدی برای پردازش سند ارسال می‌شود و این بار تقاضا با نتیجه مناسب پاسخ داده می‌شود.</p>
<br><br>
<dl>
<strong>priority</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    3
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی اولویت درخواست را تعیین می‌کند. مقدار آن می‌تواند بین ۱ تا ۴ باشد. عدد کمتر نشان‌دهندهٔ اولویت بالاتر است.</p>
<br><br>
## نمونه دریافت نتیجه در حالت ناهمگام

> Request

```plaintext
{
    "task_id": "...",
    "wait": false
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "task_id": "...",
    "wait": false
} \
      https://alefba.roshan-ai.ir/api/read_document/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "task_id": "...",
    "wait": false
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/read_document/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/read_document/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "task_id": "...",
    "wait": false
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/read_document/";
  $content = json_encode(
      '{
    "task_id": "...",
    "wait": false
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/read_document/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "task_id": "...",
    "wait": false
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
  {
    "state":"started",
    "task_id": "..."
  }


```

`POST /api/read_document/`

<dl>
<strong>task_id(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    0
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  شناسه درخواست ارسال شده</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر این ویژگی فعال باشد، کاربر منتظر می‌ماند تا نتیجه تحلیل آماده شود؛ در غیر این صورت، تقاضای تحلیل دریافت می‌شود و کاربر با استفاده از واسط «وضعیت سند» از میزان پیشرفت تحلیل مطلع می‌شود. به این ترتیب پس از پایان پردازش، تقاضای جدیدی برای پردازش سند ارسال می‌شود و این بار تقاضا با نتیجه مناسب پاسخ داده می‌شود.</p>
<br><br>
## نمونه ارسال مستقیم سند

> Request

```plaintext
"--{boundary value}\nContent-Disposition: form-data; name='document'; filename='FILE NAME'\nContent-Type: text/plain (according to the type of the uploaded file)\n\n{file content}\n--{boundary value}\n"
```

```shell
curl -X
      POST --header "Authorization: Token TOKEN_KEY"
      -F "document=@example.pdf"
      http://alefba.roshan-ai.ir/api/read_document
```

```python
import requests

headers = {'Authorization': 'Token TOKEN_KEY',}
files = {'document': ('FILE NAME', open('YOUR FILE PATH', 'rb')),}
response = requests.post('https://alefba.roshan-ai.ir/api/read_document/', headers=headers, files=files)
print(response)
```

```java
import java.io.*;
import java.net.*;
import java.nio.file.Files;

public class MultiPartRequest {
  public static void main(String[] args) throws IOException {

    String url = "https://alefba.roshan-ai.ir/api/read_document/";
    File textFile = new File("YOUR FILE PATH");
    String boundary = Long.toHexString(System.currentTimeMillis());
    String CRLF = "\r\n";

    URLConnection connection = new URL(url).openConnection();
    connection.setDoOutput(true);
    connection.setRequestProperty("accept", "*/*");
    connection.setRequestProperty("Connection", "close");
    connection.setRequestProperty("Authorization", "Token TOKEN_KEY");
    connection.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + boundary);

    try (
        OutputStream output = connection.getOutputStream();
        PrintWriter writer = new PrintWriter(new OutputStreamWriter(output), true);
    ) {
      writer.append("--").append(boundary).append(CRLF);
      writer.append("Content-Disposition: form-data; name=\"document\"; filename=\"").append(textFile.getName()).append("\"").append(CRLF);
      writer.append("Content-Type: application/pdf").append(CRLF);
      writer.append(CRLF).flush();
      Files.copy(textFile.toPath(), output);
      output.flush();
      writer.append(CRLF).flush();
      writer.append("--").append(boundary).append("--").append(CRLF).flush();
    }


    BufferedReader inputStream = new BufferedReader(new InputStreamReader((InputStream) connection.getContent()));
    String inputLine;
    while ((inputLine = inputStream.readLine()) != null){
      System.out.println(inputLine);
    }
    inputStream.close();
  }
}
```

```php
$fields = array("f1"=>"value1", "another_field2"=>"anothervalue");

$filenames = array("FILE_PATH_1", "FILE_PATH_2");

$files = array();
foreach ($filenames as $f){
   $files[$f] = file_get_contents($f);
}

$url = "https://alefba.roshan-ai.ir/api/read_document/";

$curl = curl_init();

$url_data = http_build_query($data);

$boundary = uniqid();
$delimiter = '-------------' . $boundary;

$post_data = build_data_files($boundary, $fields, $files);

curl_setopt_array($curl, array(
  CURLOPT_URL => $url,
  CURLOPT_RETURNTRANSFER => 1,
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POST => 1,
  CURLOPT_POSTFIELDS => $post_data,
  CURLOPT_HTTPHEADER => array(
    "Authorization: Bearer $TOKEN",
    "Content-Type: multipart/form-data; boundary=" . $delimiter,
  ),
));

$response = curl_exec($curl);

$info = curl_getinfo($curl);
var_dump($response);
$err = curl_error($curl);
echo "error";
var_dump($err);
curl_close($curl);

function build_data_files($boundary, $fields, $files){
    $data = '';
    $eol = "\r\n";
    $delimiter = '-------------' . $boundary;
    foreach ($fields as $name => $content) {
        $data .= "--" . $delimiter . $eol
            . 'Content-Disposition: form-data; name="' . $name . "\"".$eol.$eol
            . $content . $eol;
    }
    foreach ($files as $name => $content) {
        $data .= "--" . $delimiter . $eol
            . 'Content-Disposition: form-data; name="' . $name . '"; filename="' . $name . '"' . $eol
            . 'Content-Type: image/png'.$eol
            . 'Content-Transfer-Encoding: binary'.$eol
            ;
        $data .= $eol;
        $data .= $content . $eol;
    }
    $data .= "--" . $delimiter . "--".$eol;
    return $data;
}
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Collections.Generic;
using System.Threading;

namespace MyRequest
{
    class Program
    {
		static async void UploadFile(String serverAddress,String filePath,String[] paramsName,String[] paramsValue){
			using (var formData = new MultipartFormDataContent()){
				formData.Headers.ContentType.MediaType = "multipart/form-data";
				var filestream = new FileStream(filePath, FileMode.Open);
				Stream fileStream = System.IO.File.OpenRead(filePath);
				var fileName = System.IO.Path.GetFileName(filePath);
				
				formData.Headers.ContentDisposition = new ContentDispositionHeaderValue("attachment")
				{
					FileName = fileName
				};
				
				for(int i = 0;i<paramsName.Length;i++){
					var stringContent = new StringContent(paramsValue[i]);
					stringContent.Headers.Add("Content-Disposition", "form-data; name=\"" + paramsName[i] + "\"");
					formData.Add(stringContent, paramsName[i]);
				}
				
				formData.Add(new StreamContent(fileStream), "file", filename);
				
				using (var client = new HttpClient()){
					client.DefaultRequestHeaders.Add("Authorization", "Token" + _bearerToken);
					
					var response = await client.PostAsync(serverAddress, formData).Result;
					return response.ToString();
					
					var message = await client.PostAsync(serverAddress, formData);
					result = await message.Content.ReadAsStringAsync();
					return result;
				}
			}
		}
    }
}
```

> Response 

```json
    {
      "pages": [{
        "page_url": "http://alefba.roshan-ai.ir/media/files/96/84/859267728361-boute.pdf@page=1",
        "width": 2125,
        "height": 2750,
        "text": "بوته\n\nدرس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
        "parts": [
          {
            "type": "text",
            "direction": "rtl",
            "box": "209 305 1711 449",
            "text": "درس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
            "lines": [
              {
                "probability": 1.0,
                "box": "211 305 1707 57",
                "text": "درس‌های دانشگاهی معمولا با پروژه‌هایی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها"
              },
              ...
            ]
          },
          ...
        ]
      },{
        "page_url":"http://alefba.roshan-ai.ir/media/files/96/84/859267728361-boute.pdf@page=2",
        "width": 2125,
        "height": 2750,
        "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین\n...",
        "parts": [
          {
            "type": "text",
            "direction": "rtl",
            "box": "210 110 1714 294",
            "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین\n...",
            "lines": [
              {
                "probability": 1.0,
                "box": "210 110 1706 58",
                "text": "انجام این کار البته چندان هم آسان نیست؛ نه برای دانشجوها و نه برای کمک‌استادها، اما حاصل بسیار شیرین"
              },
              ...
            ],
          },
          ...
        ]
      }],
      "document_url":"http://alefba.roshan-ai.ir/media/files/96/84/859267728361-boute.pdf"
    }


```

`POST /api/read_document/`

<dl>
<strong>document(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    file in binary
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  فایل سند ورودی</p>
<br><br>
<dl>
<strong>type</strong>
<br>
<br>
Value: 
<span className="enum-container">
<span>
general
</span>
<span style="font-family:VazirCode;">
سندهای عمومی
</span>
<span>
ID-card
</span>
<span style="font-family:VazirCode;">
کارت‌های شناسایی
</span>
<span>
excel
</span>
<span style="font-family:VazirCode;">
جدولهای مالی
</span>

</span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی نوع سند را مشخص می‌کند</p>
<br><br>
<dl>
<strong>fix_orientation</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی، الفبا سعی می‌کند چرخش ۹۰، ۱۸۰ و یا ۲۷۰ درجه تصویر را اصلاح کند</p>
<br><br>
<dl>
<strong>word_positions</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی اطلاعات مکانی واژه‌ها در خروجی قرار می‌گیرد</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر این ویژگی فعال باشد، کاربر منتظر می‌ماند تا نتیجه تحلیل آماده شود؛ در غیر این صورت، تقاضای تحلیل دریافت می‌شود و کاربر با استفاده از واسط «وضعیت سند» از میزان پیشرفت تحلیل مطلع می‌شود. به این ترتیب پس از پایان پردازش، تقاضای جدیدی برای پردازش سند ارسال می‌شود و این بار تقاضا با نتیجه مناسب پاسخ داده می‌شود.</p>
<br><br>
<dl>
<strong>priority</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    3
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی اولویت درخواست را تعیین می‌کند. مقدار آن می‌تواند بین ۱ تا ۴ باشد. عدد کمتر نشان‌دهندهٔ اولویت بالاتر است.</p>
<br><br>
# خواندن صفحه

این تابع، تصویر نوشته را دریافت می‌کند و متن آن را در قالب JSON باز می‌گرداند. خروجی نویسه‌خوان شامل بخش‌های نوشته (پاراگراف)، جدول و تصویر است. مکان و ابعاد هر بخش در خروجی مشخص شده است و اطلاعات کامل خطوط متن در بخش نوشته ظاهر می‌شود. برای هر خط متن، ویژگی احتمال صحت هم قرار داده شده که نشان می‌دهد ابزار نویسه‌خوان چقدر از نتیجه تحلیل، مطمئن است.


## نمونه

> Request

```plaintext
{
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
} \
      https://alefba.roshan-ai.ir/api/read_pages/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/read_pages/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/read_pages/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/read_pages/";
  $content = json_encode(
      '{
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/read_pages/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "page_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1"
    ],
    "fix_orientation": true,
    "word_positions": false,
    "wait": true,
    "priority": 3
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
[{
  "page_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1",
  "width": 2125,
  "height": 2750,
  "text": "بوته\n\nدرس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
  "parts": [
    {
      "type": "text",
      "direction": "rtl",
      "box": "209 305 1711 449",
      "text": "درس‌های دانشگاهی معمولا با پروژه‌هابی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها\n...",
      "lines": [
        {
          "probability": 1.0,
          "box": "211 305 1707 57",
          "text": "درس‌های دانشگاهی معمولا با پروژه‌هایی همراه هستند که سهم قابل‌توجهی از آموزش را بر عهده دارند. این پروژه‌ها"
        },
        ...
      ]
    },
    ...
  ]
}]

```

`POST /api/read_pages/`

<dl>
<strong>page_urls(required)</strong>
<br>
<br>
Value: [URL, ]
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس صفحات ورودی</p>
<br><br>
<dl>
<strong>type</strong>
<br>
<br>
Value: 
<span className="enum-container">
<span>
general
</span>
<span style="font-family:VazirCode;">
سندهای عمومی
</span>
<span>
ID-card
</span>
<span style="font-family:VazirCode;">
کارت‌های شناسایی
</span>
<span>
excel
</span>
<span style="font-family:VazirCode;">
جدولهای مالی
</span>

</span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی نوع سند را مشخص می‌کند</p>
<br><br>
<dl>
<strong>fix_orientation</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی، الفبا سعی می‌کند چرخش ۹۰، ۱۸۰ و یا ۲۷۰ درجه تصویر را اصلاح کند</p>
<br><br>
<dl>
<strong>word_positions</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این ویژگی اطلاعات مکانی واژه‌ها در خروجی قرار می‌گیرد</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر این ویژگی فعال باشد، کاربر منتظر می‌ماند تا نتیجه تحلیل آماده شود؛ در غیر این صورت، تقاضای تحلیل دریافت می‌شود و کاربر با استفاده از واسط «وضعیت سند» از میزان پیشرفت تحلیل مطلع می‌شود. به این ترتیب پس از پایان پردازش، تقاضای جدیدی برای پردازش سند ارسال می‌شود و این بار تقاضا با نتیجه مناسب پاسخ داده می‌شود.</p>
<br><br>
<dl>
<strong>priority</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    3
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  این ویژگی اولویت درخواست را تعیین می‌کند. مقدار آن می‌تواند بین ۱ تا ۴ باشد. عدد کمتر نشان‌دهندهٔ اولویت بالاتر است.</p>
<br><br>
# وضعیت سند

این تابع برای هر کدام از سندهای ورودی مشخص می‌کند که چه تعداد از صفحات آنها تحلیل شده است.


## نمونه

> Request

```plaintext
{
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
} \
      https://alefba.roshan-ai.ir/api/document_status/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/document_status/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/document_status/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/document_status/";
  $content = json_encode(
      '{
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/document_status/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_urls": [
        "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
    ]
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
{
  "http://bayanbox.ir/view/5067853395275628881/boute.pdf": {
    "analyzed": true,
    "processed_pages": 2,
    "all_pages": 2
  }
}

```

`POST /api/document_status/`

<dl>
<strong>document_urls(required)</strong>
<br>
<br>
Value: [URL, ]
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس اسناد ورودی</p>
<br><br>
# صفحات سند

نویسه‌خوان برای تحلیل سند، ابتدا باید آن را صفحه‌صفحه کند. این تابع، فایل سند را در قالب PDF دریافت می‌کند و صفحات آن را به عنوان نتیجه باز می‌گرداند. بعد از این مرحله، تابع «خواندن صفحه» می‌تواند هر کدام از صفحه‌های سند را تحلیل کند.


## نمونه

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
} \
      https://alefba.roshan-ai.ir/api/document_pages/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/document_pages/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/document_pages/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/document_pages/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/document_pages/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
{
  "document_url":"http://bayanbox.ir/view/5067853395275628881/boute.pdf",
  "pages": [
    "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=1",
    "http://bayanbox.ir/view/5067853395275628881/boute.pdf@page=2"
  ]
}

```

`POST /api/document_pages/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
# دریافت فایل ورد

با استفاده از این تابع می‌توانید نتیجه صفحات پردازش شده را در قالب فایل Microsoft Word دریافت نمایید. در فایل خروجی، قالب سند حفظ شده است و اجزای متن شامل خطوط، پاراگراف‌ها و خانه‌های جدول در جای خود قرار گرفته‌اند.


## نمونه

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
} \
      https://alefba.roshan-ai.ir/api/download_word/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/download_word/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/download_word/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/download_word/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/download_word/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
boute.docx file

```

`POST /api/download_word/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
# دریافت فایل اکسل

با استفاده از این تابع می‌توانید نتیجه صفحات پردازش شده را در قالب فایل Microsoft Excel دریافت نمایید. دقت کنید که برای استفاده از این خروجی لازم است در هنگام خواندن سند، ویژگی <code>type</code> را برابر مقدار <code>excel</code> قرار دهید.


## نمونه

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
} \
      https://alefba.roshan-ai.ir/api/download_excel/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/download_excel/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/download_excel/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/download_excel/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/download_excel/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
boute.xlsx file

```

`POST /api/download_excel/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
# دریافت فایل قابل جستجو

با استفاده از این تابع می‌توانید نتیجه صفحات پردازش شده را در قالب فایل PDF دریافت نمایید. فایل خروجی، تصاویر صفحات سند ورودی را به همراه نتیجه پردازش آنها نشان می‌دهد. به این ترتیب با جستجوی یک عبارت در این فایل، واژه‌های مورد جستجو در تصویر مشخص و رنگی می‌شوند.


## نمونه

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
} \
      https://alefba.roshan-ai.ir/api/download_pdf/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/download_pdf/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/download_pdf/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/download_pdf/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/download_pdf/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf",
    "quality": 100,
    "img_format": "png",
    "color": false
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
boute.pdf file

```

`POST /api/download_pdf/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
<dl>
<strong>quality</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    100
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  مقدار dpi کیفیت صفحات</p>
<br><br>
<dl>
<strong>color</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    false
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  در صورت فعال بودن این خصوصیت، فایل خروجی رنگی خواهد بود.</p>
<br><br>
# حذف سند

با استفاده از این تابع می‌توانید سند ثبت شده در سامانه را به طور کامل حذف کنید.


## نمونه

> Request

```plaintext
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary {
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
} \
      https://alefba.roshan-ai.ir/api/delete_document/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('https://alefba.roshan-ai.ir/api/delete_document/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/delete_document/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/delete_document/";
  $content = json_encode(
      '{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/delete_document/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://bayanbox.ir/view/5067853395275628881/boute.pdf"
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
{
  "message":"Document deleted successfully."
}

```

`POST /api/delete_document/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
# خواندن سند نامتقارن

این قابلیت برای جلوگیری از انتظار برای تحلیل فایل قرار داده شده است. پس  از ارسال رکوئست وضعیت فایل در حال تحلیل قرار گرفته و پس اتمام تحلیل رکوئست کالبک حاوی اطلاعات فایل به آدرس وارد شده فرستاده میشود.


## نمونه

> Request

```plaintext
{
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" --header "Content-Type: application/json" \
      --data-binary {
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
} \
      https://alefba.roshan-ai.ir/api/read_document/
```

```python
from urllib2 import Request, urlopen

values = """
{
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
}
"""

headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY','Content-Type': 'application/json',
}
request = Request('https://alefba.roshan-ai.ir/api/read_document/', data=values, headers=headers)

response_body = urlopen(request).read()
print(response_body)
```

```java
import java.lang.System;
import java.net.HttpURLConnection;
import java.io.OutputStream;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.net.URLConnection;
import java.io.InputStreamReader;
import java.io.BufferedReader;

class MyRequest {

    public static void main(String[] args){
        try{
            URL url = new URL("https://alefba.roshan-ai.ir/api/read_document/");
            URLConnection con = url.openConnection();
            HttpURLConnection http = (HttpURLConnection)con;
            http.setRequestMethod("POST");
            http.setDoOutput(true);

            byte[] out = "{
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
}".getBytes(StandardCharsets.UTF_8);
            int length = out.length;

            http.setFixedLengthStreamingMode(length);
            http.setRequestProperty("Content-Type", "application/json");
            http.setRequestProperty("Authorization", "Token TOKEN_KEY");
            http.setRequestProperty("Content-Type", "application/json");
            http.connect();
            try(OutputStream os = http.getOutputStream()) {
                os.write(out);
            }

            BufferedReader in = new BufferedReader(new InputStreamReader(http.getInputStream()));
            String inputLine;
            while ((inputLine = in.readLine()) != null)
                System.out.println(inputLine);
            in.close();
        }
        catch(Exception e){
            System.err.println(e);
        }
    }
}
```

```php
<?php

  $url = "https://alefba.roshan-ai.ir/api/read_document/";
  $content = json_encode(
      '{
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
}');
  $curl = curl_init($url);
  curl_setopt($curl, CURLOPT_HEADER, false);
  curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($curl, CURLOPT_HTTPHEADER,
          array(
              "Content-Type: application/json",
              "Authorization: Token TOKEN_KEY",
              "Content-Type: application/json",
              );
  curl_setopt($curl, CURLOPT_POST, true);
  curl_setopt($curl, CURLOPT_POSTFIELDS, $content);

  $json_response = curl_exec($curl);

  $status = curl_getinfo($curl, CURLINFO_HTTP_CODE);

  if ( $status != 200 ) {
      die("Error: call to URL $url failed with status $status, response $json_response, curl_error " . curl_error($curl) . ", curl_errno " . curl_errno($curl));
  }


  curl_close($curl);

  $response = json_decode($json_response, true);
?>
```

```csharp
using System;
using System.IO;
using System.Net;
using System.Collections.Generic;

namespace MyRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://alefba.roshan-ai.ir/api/read_document/");
            httpWebRequest.Headers["Content-Type"]= "application/json";
            httpWebRequest.Headers["Authorization"]= "Token TOKEN_KEY";
            httpWebRequest.Headers["Content-Type"]= "application/json";

            httpWebRequest.Method = "POST";

            using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
            {
                string json = "{
    "document_url": "http://alefba.roshan-ai.ir/media/files/46/00/166072370361-internship.pdf",
    "callback_url": "http://192.168.254.3:5013/alefba_post"
}";

                streamWriter.Write(json);
                streamWriter.Flush();
                streamWriter.Close();
            }

            var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
            using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
            {
                var result = streamReader.ReadToEnd();
                Console.WriteLine(result);
            }
        }
    }
}
```

> Response 

```json
{
  "state":"processing",
  "document_url":"http://192.168.254.3:5013/static/2021-10-12%20%281%29.png"
}

```

`POST /api/read_document/`

<dl>
<strong>document_url(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس سند ورودی</p>
<br><br>
<dl>
<strong>color(required)</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    URL
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس بازگشت بعد از تمام شدن فرآیند خواندن سند</p>
<br><br>
