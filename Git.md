# Git skills
## Dump a specific commit of a specific file

```
git ls-tree <hash> [<path>]      # findout the file-SHA1
git cat-file blob <file-SHA1>
```

Another example:

```
git show 27cf8e84bb88e24ae4b4b3df2b77aab91a3735d8:full/repo/path/to/my_file.txt
```

