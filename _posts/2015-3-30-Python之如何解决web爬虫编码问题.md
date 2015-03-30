---
layout: post
title: Python之如何解决web爬虫编码问题
---

最近写扫描器了，在获取site title的时候遇到了一个问题，那就是不同site的charset是不同的，因此，在使用mechanize的时候，
###
    br = mechanize.Browser()
    print br.title
之后会出现编码问题，导致了乱码现象。
在网上找了一大堆的解决方案，还是不能完美解决。于是没办法，自己动手丰衣足食。
解决方案：
获取html的charset信息，然后再decode().encode()
带码如下：
###
    html = br.open(url).read()
    code = re.search(r"(charset=["])([a-zA-Z0-9-])(["])", html)
    if code:
        code = code.group(2)
    else:
        code = 'utf-8'
        title = br.title()
        title = title.decode(code).encode('utf-8')
    print title

但是实战的时候并非如此，还是会出现一些问题。于是将代码做了如下改动，排除mechanize的问题之外，准确率达到了百分之九十八以上了。
###
    html = br.open(url).read()
    code = re.search(r"(charset=["])([a-zA-Z0-9-])(["])", html)
    if code:
        code = code.group(2)
    else:
        code = 'utf-8'
    title = br.title()
    try:
        title = title.decode(code).encode('utf-8')
    except:
        try:
            if code is not'utf-8':
            title = title.decode('gbk').encode('utf-8')
        except BaseException, e:
        pass
    print title, url,
