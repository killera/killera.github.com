---
category: python
layout: post
tags: [BeautifulSoup]
---

[BeautifulSoup][beautifulsoup]是一个处理HTML、XML的python。它可以用“.”来访问HTML的元素。

下面是一段python 脚本， 用于从TED网站抓取视频字幕：


    from urllib2 import urlopen
    from BeautifulSoup import BeautifulSoup
    import sys


    def main():
        if 2 > len(sys.argv):
            print 'python {0:s} <ted_url>'.format(__file__)
            return

        url = sys.argv[1]
        print url
        soup = BeautifulSoup(urlopen(url).read())
        attr_map = soup.find(id="share_and_save").attrMap
        langs = ['en', 'zh-cn']
        for lang in langs:
            subtitle_url = 'http://www.ted.com/talks/subtitles/id/%s/lang/%s/format/html' % (attr_map['data-id'], lang)
            beautiful_soup = BeautifulSoup(urlopen(subtitle_url).read())

            subtitle = beautiful_soup.getText("\n").encode('utf-8')

            with open("%s-%s.txt" % ((attr_map['data-title']), lang), 'w') as f:
                f.write(subtitle)
    if __name__ == '__main__':
        main()


将上述代码存为ted.py, 在命令行键入：`python ted.py <ted_video_url>`


[beautifulsoup]: http://www.crummy.com/software/BeautifulSoup/bs4/doc/