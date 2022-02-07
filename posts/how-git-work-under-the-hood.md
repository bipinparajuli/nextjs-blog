---
title: "how-git-work-under-the-hood"
date: "2022-02-07"
---

# Motivation

This blog does not focus more on git command but actually how git work. Many of devs including myself at least on the starting point we memorize some commands like git add, git push, git commit etc. we try to make our job done without properly understanding how actually these things are working. If we face any error we even don't know where these error are coming from then we remove complete .git folder and start working again on new repo.  

## How git store information?

At a core level git store its information in key value pair. Value is actual data and key is the hash of the data, key is 40 character long SHA1 hash value which is a unique number. We can easily retrive data by using key.

## key


![alt text for screen readers](/images/hash.png "Text to show on mouseover")

We can get hash of the content even before writing it using git hash-object --stdin

## Value

Git store the compressed data in a blob inside object along the meta data in the header. The way git stores these blob under the hood is by calling command "git hash-object" to generate hash of the given data.Blob are generally unique in git.
 Meta data of blob includes:
 - the identifier 
 - size of the blob
 - delimiter
 - content

 Git stores filename and directory strucutre in tree. Tree is a pointer using(SHA1) which points to blob and other trees. Tree also stores matadata what type of pointer(blob/tree), filename or directory name and mode(executable file,symbolic link)

 Let's understand one by one through example:

Create a file hello.txt and put simple text hello world inside of hello.txt  

![alt text for screen readers](/images/hello.png "Text to show on mouseover")

Then let's add this file to staging area and commit this file.

Git stores snapshot of the commit inside .git directory. if we inspect .git directory we will get the following result.

![alt text for screen readers](/images/trees.png "Text to show on mouseover")

hmm!!! confusing right?

let's inspect one by one. Git stores contents under a file in the object directory with the object-id as a name. We can see 3 directory commit,tree and blob starting with first two character of respective hash. We can read the type and content by using git cat-file with -t or -p flag to read type and content respectively. We starting to inspect commit as first.

![alt text for screen readers](/images/tcommit.png "Text to show on mouseover")

![alt text for screen readers](/images/commit.png "Text to show on mouseover")


Git commit makes a commit out of the changes in the staging area and updates the reference of the branch of that commit. As said -t gives the type to commit. Commit points to tree, link to the parent commit if present and contain metadata:

- author
- date
- messege
- parent commit (one or more)

Now, inspect the pointer to tree 

![alt text for screen readers](/images/ttree.png "Text to show on mouseover")

![alt text for screen readers](/images/tree.png "Text to show on mouseover")

We can see the type to the tree and as we discuss above tree contain:

- mode : 100644
- type : blob
- pointer to blob or tree -557db03de997c86a4a028e1ebd3a1ceb225be238
- filename : hello.txt

Now, let's see last one:

![alt text for screen readers](/images/tblob.png "Text to show on mouseover")

![alt text for screen readers](/images/blob.png "Text to show on mouseover")

Now, it has been easy right we can see type blob as aspected and our content hello world .


## Branch

Branch lives in refs/head inside .git directory. Our current branch points to the hash of initial commit. .git/refs/heads/masters contain which commit the branch points to. HEAD is usally pointer to the current branch.

![alt text for screen readers](/images/pointer.png "Text to show on mouseover")


THANKS FOR READING.


