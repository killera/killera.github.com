---
title: Build hostsman step by step  
category: python  
tags: [python, pip, pypi]  
layout: post  
---

Hostsman is a command line tool for managing the hosts file, it is written in python, thus a cross-platform tool. Here is the basic usage:
![image](/assets/images/hostsman-help.png)

For more detailed usage, please check the [Wiki page](https://github.com/qszhuan/hostsman/wiki/Welcome-to-the-hostsman-wiki!).

## Motivation

It's all because I want to save some time on editing the hosts file. As for the programming language, python is good for building cross-platform tools. Also `pip` is very easy to install a python package, and of course a tool.

## Requirement
I think about serveral basic needs, like viewing the file, adding new mapping, deleting a mapping, checking a hostname. 

I want it to be a command line tool, with a colorful output.

The installation way is using `pip` or `easy_install`, as python and pip should already be preinstalled on a developer/administrator's environment.

It should be compatible for different python version, such as python2.7, python3.5, ...

That's it.

## Development

### Step 1 - Main logic

Add a hostsman.py file and implement the code to read and write hosts file. Please check the  [code here](https://github.com/qszhuan/hostsman/blob/master/hostsman.py#L12-L78) if interested.

### Step 2 - Command line arguments handling

Make it a command line script with `argparse`:

Here is the code:


{% highlight python %}
def init_parser(file_path):
    parser = argparse.ArgumentParser(description='add, remove or list mappings in hosts file',
                                     epilog='hosts file location: ' + file_path)
    group = parser.add_mutually_exclusive_group()
    group.add_argument("-l", "--list", action="store_true", help="Show the content of hosts file")
    group.add_argument("-c", "--check", metavar='HOSTNAME', nargs='+',
                       help="Check if the host name existed in the host file")
    group.add_argument("-i", "--insert", metavar='HOSTNAME[:IP]', nargs='+',
                       help="Insert HOSTNAME[:IP] mappings")
    group.add_argument("-r", "--remove", metavar='HOSTNAME', nargs='+',
                       help="Remove mapping for HOSTNAME from hosts file.")
    return parser
{% endhighlight %}

Then connect it up with the code:

{% highlight python %}
def main():
    host = Host()
    parser = init_parser(host.location())
    args = parser.parse_args()

    if args.list:
        pass
    elif args.check:
        pass
    elif args.insert:
        pass
    elif args.remove:
        pass
    else:
        parser.print_help()
{% endhighlight %}

### Step 3 - Highlighting

In order to make the output more friendly, I decide to use pygments to do the highlighting.

{% highlight python %}
def highlight_line(content):
    return highlight(content, PythonLexer(ensurenl=False), TerminalTrueColorFormatter())


def print_highlight(*a_list):
    for each in a_list:
        print(highlight_line(each))
{% endhighlight %}

### Step 4 - Build and publish package

We already have finished all the logic code, it's time to build a pacakge and publish it.

Check [https://packaging.python.org/en/latest/distributing.html](https://packaging.python.org/en/latest/distributing.html) for all the information.

We summarize the main steps here:

1. Add initial files, such as setup.py, setup.cfg, MANIFEST.in, README.md, LICENSE.
2. Packaging. `python setup.py sdist` for source distribution or `python setup.py bdist_wheel` for wheels.
3. Create [PyPI](https://pypi.python.org/pypi) account.(for the first time)
4. Register Hostsman.`twine register dist/hostsman-1.0.0-py2.py3-none-any.whl`
5. Upload package. `twine upload dist/*`

### Step 5 - Done, try to install

Use `pip` to install `hostsman`:

`pip install hostsman`,

Then you can use `hostsman` in command line. 
![image](/assets/images/hostsman-usage.png)

## CI

I added some tests, and want to make sure them pass on different python versions. It's very easy with travis-ci, so the test can run for every commit.

![image](/assets/images/travis-ci-hostsman.png)