---
layout: post
category : python 
tags : [testing, python, lxml, tutorial]
---

This is a simple test of Jekyll's templating

## Overview 

### Some code:

lxml_make_links_absolute.py


### Some embedded code


    #!/usr/bin/python

    import sys
    import urllib
    import optparse

    from lxml.html.diff import htmldiff, html_annotate
    from lxml.html import parse, tostring, fromstring


    parser = optparse.OptionParser()

    parser.add_option(
        '-o', '--output',
        action="store_true",
        dest="output",
        help="output file")
    parser.add_option(
        '-l', '--links',
        action="store_true",
        dest="showlinks",
        help="show the links")

    parser.add_option(
        '-d', '--diff',
        action="store_true",
        dest="diff links",
        help="diff relative vs absolute links")


    def process_input(filearg):
        if filearg and filearg != "-":
            try:
                doc = parse(filearg).getroot()
            except Exception, e:
                sys.stderr.write('exception: %s' % str(e))
                try:
                    doc = fromstring(urllib.urlopen(filearg).read(), base_url=filearg)
                except Exception, e:
                    sys.stderr.write('exception: %s' % str(e))
        else:
            doc = fromstring(sys.stding.read(), base_url=filearg)
        return doc


    def show_links(node):
        for element, attribute, link, pos in node.iterlinks():
            if attribute  == "href":
                print link


    def diff_html(node):
        before = tostring(node)
        node.make_links_absolute()
        after = tostring(node)
        return htmldiff(before,after)

    def main():
        """main function"""

        options, args = parser.parse_args()

        filearg = args[0]

        doc = process_input(filearg)

        doc.make_links_absolute()

        print tostring(doc)



    if __name__ == '__main__':
        main()

