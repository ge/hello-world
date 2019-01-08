# ge's notes on the hello-world project

I like this project because it uses the simplest starter project of all without going back to basics that are too basic for me.

The tutorial that this is based on starts [here](https://www.gatsbyjs.org/tutorial/part-zero/).

## Install node.js

I did this quite a while ago.  It was very easy, especially after trying to install jekyll.

## Install git

Install git itself.

The GitHub Desktop can be downloaded from [here](https://desktop.github.com/).  It's quite nice: it already sees this file.

How to set up a repository for the project I'm about to create: [link](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)

I created the repository from my homepage on github, following their advice: *To avoid errors, do not initialize the new repository with README, license, or gitignore files*.

I couldn't figure out how to change the gatsby project so that I could work on it, so I deleted its ownership:

$ /bin/rm -rf .git
$ git status
fatal: not a git repository (or any of the parent directories): .git
$ git init
Initialized empty Git repository in /Users/ge/Dropbox/jekyll/gatsby/hello-world/.git/

### Uh Oh!

$ git remote add origin git@github.com:ge/hello-world.git
$ git push -u origin master
The authenticity of host 'github.com (192.30.253.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

### Make new ssh key


$ ssh-keygen -t rsa -b 4096 -C "zabouti@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/ge/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/ge/.ssh/id_rsa.
Your public key has been saved in /Users/ge/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:dEnqzaQrMYH/Mvm+Wz3qsl+aZBVTgYaU5GZgKedfWWE zabouti@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|        o++o .Eo |
|     ...o=o.oo.  |
|    . .+o B.oo   |
|     . +.O  oo   |
|      + S.o..    |
|       = ..o     |
|      = o + +    |
|       =.+ = .   |
|       .*B*      |
+----[SHA256]-----+
$ eval "$(ssh-agent -s)"
Agent pid 10863

### copy PUBLIC key to clipboard

>$ $ pbcopy < ~/.ssh/id_rsa.pub

### Now!

[Add new SSH key to Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

>$ git push -u origin master
>Counting objects: 9, done.
>Delta compression using up to 12 threads.
>Compressing objects: 100% (8/8), done.
>Writing objects: 100% (9/9), 4.00 KiB | 4.00 MiB/s, done.
>Total 9 (delta 0), reused 0 (delta 0)
>To github.com:ge/hello-world.git
>* [new branch]      master -> master
>Branch 'master' set up to track remote branch 'master' from 'origin'.

### Github sent me email with my public key

>tecumseh
>f9:1e:d0:12:3b:c8:fc:15:0f:ac:ff:72:47:d1:92:40

## Install gatsby-cli

>$ sudo npm install --global gatsby-cli

## Install the gatsby starter

>$ gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world

–– LICENSE
–– README.md
–– yarn.lock
–– package–lock.json
–– package.json
–––– static
–––––– favicon.ico
–– src
–––– pages
–––––– index.js

Notice that the *node_modules* directory doesn't even exist yet.  This is because there are no plugins installed yet, I think.

## Try to run gatsby

> $ gatsby develop
> error There was a problem loading the local develop command. Gatsby may not be installed in your site's "node_modules" directory. Perhaps you need to run "npm install"? You might need to delete your "package-lock.json" as well.
> $ sudo npm install

This did a bunch of stuff.  *Now* I have a *node_modules* directory with a bunch of stuff.

## And now gatsby will run


````text
$ curl http://localhost:8000
<!DOCTYPE html><html><head><meta charSet="utf-8"/><meta http-equiv="x-ua-compatible" content="ie=edge"/><meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no"/><script src="/socket.io/socket.io.js"></script></head><body><div id="___gatsby"></div><script src="/commons.js"></script></body></html>
````

## Components

If I understand [this](https://www.gatsbyjs.org/tutorial/part-one/#building-with-components) correctly - it says "Any React component defined in src/pages/*.js will automatically become a page."  Note that it does not say that you have to define components in *src/components*.

I suspect that the *export* keyword helps define components??

They are [React components](https://www.gatsbyjs.org/tutorial/part-one/#building-with-components) written in *JSX*, I think.

### Sub-components - building blocks for other things

I think that things in the *src/components* directory are there simply to keep them from being available as pages.  Otherwise they're no different from stuff in *src/pages*.

If you look at the sub-component, *src/components/header.js*, you'll see this simple file:

````text
import React from "react"

export default () => <h1>This is a header.</h1>
````

Then I can just use it in a file in *src/pages*:


````text
import Header from "../components/header"

<Header />
````

If I remove *export* from *header.js*, it cannot be imported into the other files.

Notice that these components appear to be javascript functions.

### props: things get confusing

Note the word *props* in this rewritten version of *src/components/header.js*:

````text
import React from "react"

export default props => <h1>{props.headerText}</h1>
````

I must say that I don't understand the syntax of this: *props* apparently means the same thing on both sides.  The word itself doesn't matter: it will work if I change both instances to, say, *args*, but they both must be the same.

Now I can pass an argument named *HeaderText* to the Header component:


````text
<Header HeaderText="This is Strange" />
````

Notice that case doesn't seem to matter.

### Wisdom from Justin

I asked him about:

````text
export default props => <h1>{props.headerText}</h1>
````

````text
That is ES6 AND React syntax (they call it JSX)
export default props => is the ES6 part.  It defines a new anonymous function that takes one argument called "props"
Export default basically says expose this function as the default object
In the react world "props" are the arguments you feed into a component.  Props is an object
The <h1> html looking tag stuff is JSX which is pretty much the same as html with a few editions
Default is indeed the name, but's sort of a special name
If you do const foo = require('thatfile')
You will end up with foo.default being that function
````
