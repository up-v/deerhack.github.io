---
layout: post
title: Google rank by Python
---

  当你学会如何批量之后，随之而来的问题是如和批量查询这些站点的rank,一开始的时候是在一些站长工具里面查询，
  后来发现这样极不方便，效率也低。后来就上网找rank的接口，然后发现了个googlerank.贴一下我的代码，大家参考下吧。
###
    
  def HashURL (query):
    
    SEED = "Mining PageRank is AGAINST GOOGLE'S TERMS OF SERVICE. Yes, I'm talking to you, scammer."
    Result = 0x01020345
    
    for i in range(len(query)) :
        Result ^= ord(SEED[i%len(SEED)]) ^ ord(query[i])
        Result = Result >> 23 | Result << 9
        Result &= 0xffffffff
    
    return '8%x' % Result

  def pagerank(domain):  

    StartURL = "http://toolbarqueries.google.com/tbr?client=navclient-auto&features=Rank:&q=info:";
    GoogleURL = StartURL+domain+'&ch='+HashURL(domain)
     
    try:
        rank =  urllib.urlopen(GoogleURL).read()
        if rank == "":
            rank = "error"
        return domain, rank,
    
    except BaseException, e:
    
        print  e, domain
        return domain, "error"

  if __name__ == "__main__":
    urls = open("url.txt", "r").readlines()
    urls = urlDomain(urls)
    for url in urls:
        domain, rank = pagerank(url)
        print rank, domain,
这里的HashURL是google给定的一个计算hash值得算法。
