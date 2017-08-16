---
title: Build hostsman step by step  
category: python  
tags: [python, pip, pypi]  
layout: post  
lang: en
---

`Hostsman` is a command line tool for managing the hosts file, it is written in python, thus a cross-platform tool. Here is the basic usage:
![image](/assets/images/hostsman-help.png)

For more detailed usage, please check the [Wiki page](https://github.com/qszhuan/hostsman/wiki/Welcome-to-the-hostsman-wiki!).

## Motivation

It's all because I want to save some time on editing the hosts file. As for the programming language, python is good for building cross-platform tools. Also `pip` is very easy to install a python package, and of course a tool.

## Requirement

1. Serveral basic features: viewing the hosts file, adding new mapping, deleting a mapping, checking the existnce of a hostname. 
2. For safty, it will generate a backup file before any adding/deleting operation.
3. It will be a command line tool, with a colorful output. The user will have consistent view of the output, no matter he/she is using powershell, cmd, or shell.
4. Use `pip` or `easy_install` to install it, as python and pip should already be preinstalled on a developer/administrator's environment.
5. It will be compatible for all main python versions, such as python2.7, python3.5, ...

That's it.

## Development

### Step 1 - Main logic

The main logic locates in file `hostsman.py`, which is encapsulated in class `Host`. Please check the [code here](https://github.com/qszhuan/hostsman/blob/master/hostsman.py#L16-L113) if interested.

### Step 2 - Command line arguments handling

Use python's user-friendly command line interface `argparse`:

There are several commands:

1. `-l`, `--list`  : Show the content of hosts file.
1. `-c`, `--check` : Check if a host name is existed in the hosts file.
1. `-i`, `--insert` : Insert HOSTNAME[:IP] mapping into hosts file.
1. `-r`, `--remove` : Remove the mapping for HOSTNAME from the hosts file.


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


The code above is the first version. There was an issue to show the color in Windows command line window, and the PythonLexer is not ideal to handle the highlight for different elements in the hosts file (such as comment, host name, ip address). So, I made two changes afterwards:

#### HostsLexter
I didn't find a lexter for hosts file, so I created one. Actually it is very easy. Just follow the documentation of pygments. Here is the code: 

{% highlight python %}
class HostsLexer(RegexLexer):
    name = 'Lexer for hosts file'
    aliases = ['hosts']
    filenames = ['hosts']

    tokens = {
        'root': [
            (r'#.*?$', Name.Decorator),
            (r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b', Name.Class),
            (r'[A-Fa-f0-9]{0,4}:{1,2}[A-Fa-f0-9]{1,4}%?\w*\b', Name.Class),
            (r'.+', Name.Attribute)
        ]
    }
{% endhighlight %}

#### Solve the pygment issue on `cmd.exe`

After spike, python library `curses` seems to be the one to solve this problem. I also remember that `httpie` has colorful output in terminal. After check its code, I found it already has a very decent way to handle this. So I borrowed this part of code from it. Thanks.

After the changes, the output on mac system is:
![image](/assets/images/hostsman.highlights.v2.png)

On Windows powershell:
![image](/assets/images/hostsman.ps.png)

on Windows cmd:
![image](/assets/images/hostsman.cmd.png)


### Step 4 - Build and publish package

This is the last step, try to build a package and publish it to `pypi`, so everyone can download and install it. Without any doubt, always the python `setuptools`.

Please check [https://packaging.python.org/en/latest/distributing.html](https://packaging.python.org/en/latest/distributing.html) for details.

The main steps are as follow:

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

I added some tests, and want to make sure them pass on different python versions. It's very easy with `travis-ci`, so the test can run for every commit.

![image](/assets/images/travis-ci-hostsman.png)