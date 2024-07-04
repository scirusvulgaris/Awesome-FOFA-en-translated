Sure, here is the translated content for your GitHub page:

# What are Rules?

We often mention a concept called key characteristics. When you have a key characteristic that you need to search for information on, you can find it using FOFA, such as IP addresses, domain names, website titles, website icons, etc.

So, what has FOFA done with these characteristics? We have combined and organized multiple key characteristics of these assets to form rule fingerprint sets.

In simple terms, we have created navigation pages for a lot of data, or you can think of them as the table of contents of a book. This helps users quickly and accurately find the information they need. For example, if you want to search for a certain brand of camera, FOFA has already organized the characteristics of these cameras into navigation tags. You just need to enter "Hikvision camera" in the search box to find the information on various brands of cameras that FOFA has organized.

## Usage Scenarios

The core capability of the fingerprint rule set is to help users sort out assets without manually collecting asset characteristics; you can directly search for the rule name. During the search, FOFA will provide related recommendation functions. Moreover, users can create custom rules for use, turning complex query syntax into a simple rule for direct use. Any custom rule that passes the platform review will grant the creator different amounts of F coins based on its importance.

As shown below, you can directly perform a fuzzy search for the corresponding rules:

![1](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture1.png)

![2](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture2.png)

The specific component layers are divided into the following five categories, including both software and hardware aspects:

|Category|Examples|
|:----|:----|
|Application|Software systems, etc.|
|Support|Frameworks, middleware, etc.|
|Service|Web servers, databases, servers, etc.|
|Operating System|Linux, Windows, Unix, etc.|
|Hardware|Huawei switches, TP-Link routers, Brother printers|

FOFA rules also appear under the **IP Aggregation** button in the lower-left corner of each data item. Querying a data item allows you to access all related data with one click based on the rule associations, as shown below:

![3](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture3.png)

As seen in this data, there are related rules for application, support, service, and system categories because it is website data; thus, no hardware rules are applicable. Why make this distinction?

### Types of Rules

There are two types of rules: **Service Rules** and **Website Rules**. An IP address may bind multiple hosts, and the content of each host may not be entirely consistent.

The distinction is based on the different return information of the protocol, showing the specific differences.

|Rule Category|Difference|
|:----|:----|
|Service|All protocols are referred to as services, and HTTP/HTTPS protocols do not include HTML source code.|
|Website|HTTP/HTTPS packet capture includes HTML source code and parses other fields.|

In FOFA, there are three intuitive ways to distinguish between the two types of rule data:

**Method 1:** Use type and && (AND) in the query keywords:

    * "keyword" && type="subdomain" results in website data;
    * "keyword" && type="service" results in service data.
For example, with port=80:

port="80" && type="service" results in service data.

port="80" && type="subdomain" results in website data.

**Method 2:** In the upper right corner of the search results, you can see the type distribution list. Note that in searches, protocol=service. An example of search results for port=80 is shown in Figure 5:

![4](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture4.png)

ps: If the left list is not displayed, click the all button in the upper right corner.

**Method 3:** Check if there is a **Website Content** button under each data item. If there is a button, the data is website data. If not, it is service data.

Showing website data, type=subdomain

![5](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture5.png)

Showing protocol service data, type=service

![6](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture6.png)

# Extractable Fields for Rules

In FOFA's query syntax, only some syntax can be used for rule field extraction.

Service includes three fields, while the website includes five fields, as shown below:

|Service|Website|
|:----|:----|
|banner: version information|server: a field in the header indicating the corresponding server. server efficiency > header|
|cert: certificate|title: title extracted from the webpage HTML, highly valuable|
|protocol: protocol|body: main content extracted from the webpage HTML|
|    |header: HTTP/HTTPS protocol response header|
|    |cert: certificate|

Note that all rules are based on vendors as the standard, so fields related to individual objects or geographic locations cannot be used to record rules.

## Services/Websites with Many Rules

The following is a ranking based on the number of rules:

* snmp, PJL: Most rules, including device type, model information, etc.
    * telnet, rtsp, pptp, smtp, pop3, imap: Have rules but relatively few.
        * ssh, ftp: Some rules, but mostly none.
            * Others: Can be largely ignored.
Most of the rules are still within websites, which can be checked directly as subdomains without considering service.

### Term Parsing

|Term|Full Name|Chinese Name|
|:----|:----|:----|
|snmp|Simple Network Management Protocol|简单网络管理协议|
|PJL|Printer Job Language|打印机作业语言|
|telnet|—|远程终端协议|
|rtsp|Real Time Streaming Protocol|实时流传输协议|
|pptp|Point-to-Point Tunneling Protocol|点对点隧道协议|
|smtp|Simple Mail Transfer Protocol|简单邮件传输协议|
|pop3|Post Office Protocol-Version 3|邮局协议版本3|
|imap|Internet Message Access Protocol|交互邮件访问协议|
|ssh|Secure Shell|安全外壳协议|
|ftp|File Transfer Protocol|文件传输协议|

# Rule Extraction

This section introduces the extraction process for different rule types.

## Service Rule Extraction Process

1. Extract the "main keyword" based on different protocols (containing multiple fields).

2. **Analyze whether this "keyword" contains extractable information (type, vendor, and product type fields).**

    1. Yes, extract the information, and generate a "query expression" (e.g., banner="keyword").
    2. No (fields cannot be extracted, such as \xff\xfb\x01\xff\xfb\x03\xff\xfd\x18\xff\xfd\x1fC), skip, and end extraction.
3. Execute the "query expression" in FOFA, and check if there are results.

    1. Yes, record the number of results and proceed to step 4.
    2. No, go back to step 2.
4. Check if the results are the most numerous.

    1. Yes, proceed to step 5.
    2. No, go back to step 2.
5. Use Google or Bing search engines to confirm the type, vendor, vendor's official website, and product model.

6. Follow the rule norms and record the rule.

**Note: Steps 3 and 4 can be understood as rule correctness detection steps. In step 4, "most numerous results" means the extracted information is the optimal field, and different keywords may yield different query results.**

Finding useful key information is the starting point for rule extraction and is crucial. Next, we will focus on how to determine the keywords for service rules.

1. Confirm if there is obvious vendor information in the protocol, such as Huawei, ZTE, HIKVISION, etc.
2. Determine if there is obvious device type information in the protocol, such as router, switch, printer, etc.
3. Confirm if there is obvious device model information in the protocol, such as DocuPrint CM315/318z, F100-C-G, etc.
Most protocols contain only one key feature (also known as fingerprint), a few contain two, and very few contain three fingerprint features. Therefore, when extracting fingerprint features, you can record multiple rules based on different levels.

### Information Types and Examples for Extracting from Banner (Version Information)

1. **Contains only the device vendor name, as shown below.**

```plain
banner="MikroTik"
```
![7](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture7.png)
Through Google/Bing searches, it was found that MikroTik is a router manufacturer. This rule can confirm that the devices at these IP addresses use MikroTik routers.

2. **Contains only the product category, as shown below.**

```plain
banner="Remote Management Console"
```
![8](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture8.png)

Based on the information in the banner, it can be confirmed to be a remote management console product.

**3. Contains both the vendor name and product model, as shown below.**

```plain
banner="SHARP F0-2081"
```
Through Google/Bing searches, it was determined to be a Sharp printer with the model FO-2081.
![9](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture9.png)

### Examples of Different Protocols

Different protocols return different banner information. The following are examples of some protocols.

1. **snmp protocol (can extract more information, and most extracted information can form rules)**

Analyzing the keyword, we can extract information such as the product, vendor "Samsung", and product model "X7500GX". Based on these two pieces of information, a rule can be generated.

```plain
banner="Samsung X7500GX"
```
To confirm the correctness of the rule, query it in FOFA. This rule locates a Samsung multifunction printer (specific classification of the device requires Google/Bing search and confirmation, model X7500GX).

![10](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture10.png)

2. **telnet protocol (can extract less information, most cannot form rules)**

The banner returned information has no extractable effective information.

![11](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture11.png)

3. **telnets protocol (can extract less information, most cannot form rules)**

![12](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture12.png)

Analyzing the keyword information confirms it as a LANCOM Systems product. Google/Bing searches reveal it is a LANCOM SYSTEM VPN-ROUTER with model 1781VAW.

Rule:

```plain
banner="LANCOM 1781VAW" && protocol="telnets"
```
Note: Protocols with SSL encryption can extract information from their cert field.

4. **pptp protocol (most can only extract vendor information)**

![13](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture13.png)

Based on the content of the banner, the product vendor is DrayTek, so we can tag the IP address with DrayTek.

### **Examples of Incomplete Rule Extraction**

Incomplete rule:

```plain
banner="ZXR10 Carrier-class High-end Routing Switch of ZTE Corporation"
```

![14](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture14.png)

Analyzing this rule reveals that ZXR10 may be a model/series number of a product, containing keywords like Routing, Switch, and ZTE, indicating it might be some ZTE equipment, possibly a switch or routing.

Thus, keywords can be logically combined and confirmed in FOFA. It can be split into multiple rules.

Rule 1: routing of can confirm it is a switch.

```plain
banner="ZXR10" && banner="Switch"
```

Rule 2: routing of can confirm it is a router; since switch contains routing, we exclude it to get assets containing only routing.

```plain
banner="ZXR10" && banner="Router" && banner!="Switch"
```

**Note: Why not just use ZXR10 as a search field? Because the search results not only include switches and routers but also other equipment.**

In the entire protocol rule extraction process, the most challenging part is finding the correct keywords. This is the core of the process, with no shortcuts. It can only be mastered through repeated practice. Additionally, extensive use of search engines like Google and Bing for information gathering is essential. In summary: 1. Find the key fingerprint feature; 2. Identify the vendor; 3. Determine its classification; 4. Determine its layer.

## Web Rule Extraction Process

1. Construct the first query expression based on the search keywords;
2. FOFA search;
3. Open the webpage information corresponding to the IP address in the search results;
4. Open the source code of the webpage;
5. Extract relevant information (relevant information can accurately identify product key fields);
6. Reconstruct the query expression and execute the search again;
7. Rule correctness check;
    1. If it is a rule, proceed to step 8. Rule extraction ends.
    2. If not, repeat step 6.
8. Use Google and Bing to search for keywords, determine the product category, product model, vendor, and vendor's official website;
9. Follow the rule entry norms and record the rule.

Similarly, the most important aspect of web rules is keyword extraction. By opening the web interface, check if there is clear device type, vendor information, and device model information. After extracting the key information, use Google and Bing to search and supplement the corresponding other information.

### Practical Example of Web Rule Extraction

Search web data through FOFA and click the Web interface to open it;

![15](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture15.png)

Through the web interface, we find the keyword "AS-304T", guessing it might be a model without finding vendor and type keywords;

![16](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture16.png)

Use the keyword "AS-304T" to search in Google/Bing and find the manufacturer's official website and category;

![17](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture17.png)

Check the body source code (can be accessed via the website content button or web interface source code) and find nasModel='AS-304T' can be used as a rule feature;

![18](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture18.png)

Include the syntax in the search again to confirm the rule's accuracy, and after successful verification, record the rule.

```plain
body="nasmodel='AS-304T'"
```

![19](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture19.png)
### Examples of Rules Supported by Syntax Entry

1. **Title Website Title**

Enter in the form of title="keyword". The title may contain information that can identify the product, such as the following syntax:

```plain
title="CorCloud Industrial Equipment Remote Monitoring Management System"
```
![20](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture20.png)

A single IP address, open the webpage content.

![21](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture21.png)

Although the body contains the keyword corcloud, and there are many repeated assets/global paths, using body="corcloud" to search will hit their official website, and other keywords have assets not related to the corcloud product. The cert and header do not have identifiable places, so only the website content can identify this product.

2. **Body Website Content**

In the HTML source code of the webpage, the content between "<body>" and "</body>" accounts for more than 90%, making it an important part of analyzing rules. Rules extracted from the body cover a wide range of IPs.

Some elements' paths in the webpage, such as JS paths and in-page image paths, can also be rules.

```plain
body="/l

fradius/"
```
![22](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture22.png)

Of course, it might also be part of a website link to other URLs.

Additionally, some webpage tag attributes such as href=, src=, etc.

An extractable element is the feature of href="/themes/Gargoyle/images/favicon.png".

Special attention should be given to generating the query syntax for the body, requiring an escape backslash \ before the quotes in the content itself. Thus, the final syntax is:

```plain
body="href=\"/themes/Gargoyle/images/favicon.png\""
```

![23](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture23.png)

3. **Header Response Header**

Some features in the header response header can also extract important information.

For example, the WWW-Authenticate field, w3c authentication field, may contain vendor and product information. Or for example, Context-Type may extract the development language. Using WWW-Authenticate as an example:

```plain
header="Basic realm=\" [JouleTemp - please login as guest or admin] \""
```
![24](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture24.png)

4. **Server Service**

Some products use special servers, so the server field in the header can also be extracted. We form a separate syntax for the server because it can extract more information and is more efficient than using the header.

For example, look at the information below:

```plain
title="Sunny WebBox"
```

![25](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture25.png)

The final formed rule is:

```plain
server="Sunny WebBox" || title="Sunny WebBox"
```
5. Certificate Part

The certificate also contains many vendor and product information.

Using the following syntax for queries, the returned information has Aircontrol in the commonName field of the certificate. Through Google searches, it can be determined that this feature identifies Ubiquiti Networks, Inc.'s AirControl system, classified as industrial control.

```plain
cert="AirControl"
```

![26](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture26.png)

Compared to service rules, website rules will cover a wider range of assets due to the numerous website fields. Similarly, after identifying the vendor using the key fields, you can check other products from the same vendor and record multiple rules.

It should be noted that it is better not to use the server field for rule entry. Using the header is more accurate, such as: header="Server: LHS" || banner="Server: LHS"

Additionally, the body content of website data contains many selectable key features, which should be as standardized as possible!

For example, the search statement for nasModel='AS-304T' is: body="nasmodel='AS-304T'"

For characteristics without such key-value pairs, add a backslash (escape);

For example: body="class=\"login-nas-model\">AS-304T"

Since the content of website data rules is crucial but difficult to find, it requires a lot of time to search and continuous keyword attempts.

## Rule Deduplication

There are two ways to deduplicate, demonstrated using the previously mentioned H3C Firewall SecPath F100 as an example.

**Method 1:** Click the IP aggregation button in the lower-left corner of the data to check component layers.

![27](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture27.png)

The figure clearly shows that these two rules have been recorded in the rule library.

**Method 2:** Directly search the rules in the rule list. ps: When using the search function or entering rules, connect spaces with dashes.

![28](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture28.png)

The figure shows that this rule has already been recorded.

# Rule Entry

### Introduction to the Entry Interface

Custom rule entry location: Personal Center → My Rules → Add Rule

Items marked with * are required fields. Previously mentioned collected rule vendor and classification and its official website need to be filled in the rule entry.

Select the category of the rule you are entering, for example:

![29](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture29.png)

Record the rule based on the demonstration, as shown below:

![30](https://github.com/FofaInfo/Awesome-FOFA/blob/b17f7b806332106dd3748ec4adabbd220549d540/Storage/Rule_cheatsheet/Picture30.png)

### **Vendor Website Standard**

* If the official website can be found: use its official website URL (up to the primary domain like .com/.cn/.gov/.org), such as "[https://www.siemens.com/global/en/home.html](https://www.siemens.com/global/en/home.html)" use [https://www.siemens.com/](https://www.siemens.com/)
* If the official website cannot be found: use [https://fofa.so/](https://fofa.so/)
* Open source software: use its URL if found, otherwise use [https://fofa.so/](https://fofa.so/)
**Note: URLs should end with a slash (/)**

### Rule Name Standard

Rule name format: Company abbreviation or acronym - Product (hyphen), such as: Dahua-SD65XX HN. If there is no company abbreviation or acronym, use the full name.

	If the product model is known, use the accurate model.

	If the model is unknown, use "company product" (e.g., Huawei-company product).

### Standardization of Rule Names

1. **Rules where the correct vendor can be found**
* Vendor rule names exist in the following situations:
* If in the logo, use it directly.
* Mixed Chinese and English, use the left or top text; if the top or bottom or left or right content is part of the company as a whole, take all of it.
* Connect abbreviated words with underscores.
* If there is no logo on the official website, use the logo on the software/hardware product.
* If there is no text in the logo, use the widely known company name.
* If acquired and the official website cannot be opened or redirects, use the new company name.
* If it cannot be opened, search Bing or Google and use the vendor abbreviation shown in the wiki.
* Apache-hosted projects use the page name.
* If the logo is the company's full name, use the commonly used abbreviation.
* For acquired vendors, you can consider keeping the vendor name, but the first vendor should be the new vendor name.
2. **Rules without a vendor:**
* All open-source products use the product itself.
* If the copyright information is just the website, it is considered an open-source product.
* If the logo is one of the company's products.
* All org endings are open-source products.
* Network modules do not include company information unless explicitly stated.
**Note: In company abbreviations/acronyms/full names containing hyphens ("-"), use "_" (underscore) instead of "-".**

# Questions and Answers

**Q: What is an asset?**

A: Assets refer to software and hardware devices in the computer system, consisting of systems (like email systems), web servers/middleware (like Apache servers), operating systems (like Windows), and hardware (like Cisco routers).

**Q: What is OEM?**

A: OEM stands for "Original Equipment Manufacturer," meaning contract manufacturing cooperation, commonly known as "outsourcing." OEM products are custom-made for brand manufacturers and can only use the brand name, not the producer's name.

**Q: Why do OEM websites exist?**

A: Some enterprises do not handle part of their business themselves and outsource it to other companies. The "outsourcing company" creates a product model and modifies parts of it to achieve different display styles.

**Q: What is tagging?**

	A: Tagging generates a label for an IP address to accurately identify the contained assets. Tags can classify IP addresses, such as identifying which IP addresses contain TP-LINK AC1200 routers.

**Q: What are webpage tags?**

	A: Webpage tags are parts of the HTML source code of the webpage. For example: <title> AAA</title>, where the title tag is AAA.

**Q: What is full-text search?**

	A: Full-text search is a computer program that scans every word in an article and creates an index indicating the number and position of occurrences. When a user queries, it searches the index, similar to looking up words in a dictionary.

**Q: When analyzing website source code, I encountered partially encrypted code. How should I

 handle it?**

	A: If HTML content is encrypted (appearing irregular), extract information using JavaScript code (content within the webpage's Script tags).

**Q: I have identified a product with my rule but do not know its type, vendor, and model. Should I still enter it?**

	A: No. Rules aim to accurately identify certain business systems. Recording an unidentified product without additional information is meaningless.

**Q: I cannot determine the exact product type but can identify the vendor. Should I enter this rule?**

A: Yes. This rule can tag the IP addresses it covers with the vendor.

**Q: Why can't I find the information extracted from the banner in FOFA?**

	A: Incorrect segmentation causes this. FOFA's search engine uses dots ("."), colons (":"), hyphens ("-"), and slashes ("/") as segmentation characters. Using other characters (like underscores "_") can lead to no results.

**Q: If a product model contains a hyphen ("-"), should it be removed following the entry norms?**

	A: No. "Product-Model" hyphen is for standardizing rule names and does not affect the model.

**Q: When entering the rule name, I know the vendor but not the exact model and type. Why should I use "company product" instead?**

	A: Using "company product" standardizes the rule name format, allowing further exploration of IPs containing vendor information later.
