# Node.js

First, install the latest version of Node.js:

```
 https://github.com/nodesource/distributions#installation-instructions
```

Examples:

### EC2 Default (yum):
```
 sudo yum install gcc-c++ make
 curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash 
 sudo yum install -y nodejs
```

### Ubuntu (apt):
```
 curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
 sudo apt-get install -y nodejs
```

### Debian:
```
 curl -sL https://deb.nodesource.com/setup_10.x | bash -
 apt-get install -y nodejs
```

## Clean setup

> **Make `npm install -g` not require `sudo`.**

- Make a directory for global installations:
` mkdir ~/.npm-global`

- Configure npm to use the new directory path:
` npm config set prefix '~/.npm-global'`

- Open or create a ~/.profile file and add this line:
` export PATH=~/.npm-global/bin:$PATH`

- Back on the command line, update your system variables:
` source ~/.profile`

# Git

## Scripted Diffs

> **Quantifiable and verifiable find-and-replace.**

This technique fully leverages regular expressions and pattern matching for rich substitutions.

- [Docs from Bitcoin](https://github.com/bitcoin/bitcoin/blob/master/doc/developer-notes.md#scripted-diffs)

- [Official docs for `git-grep`](https://git-scm.com/docs/git-grep)

- [Unofficial docs for `find`](https://www.binarytides.com/linux-find-command-examples/)

- [Docs for basic vs extended regex](https://www.gnu.org/software/grep/manual/html_node/Basic-vs-Extended.html)

- [Official docs for basic regex](https://www.gnu.org/software/sed/manual/html_node/Regular-Expressions.html)

- [Unofficial docs and supertool for regex](https://regexr.com/)

- [Regex tutorial (Written)](https://github.com/ziishaned/learn-regex)

- [Regex / sed tutorials (Video)](https://www.youtube.com/user/theurbanpenguin/search?query=sed)

**NOTE:** the version of `sed` pre-installed on macOS does not cleanly support in-place editing. Install `GNU sed` for that (`brew install gnu-sed`, run via `gsed`).

**NOTE:** `bash` and other shells use 'globbing' for their wildcard behavior - this is not the same as regex, even though some symbols are shared!

### Examples -

`sed` - Replace `OLD` with `NEW`:
```
-BEGIN VERIFY SCRIPT-
git grep --files-with-matches OLD src/*.cpp | xargs sed --in-place --regexp-extended 's/OLD/NEW/g'
-END VERIFY SCRIPT-
```
~> Source: https://github.com/masonicboom/bitcoin/commit/d7ebeaf339a89077461bf32b664eed4b19ffbecb#diff-6a3371457528722a734f3c51d9238c13R136

`sed`:
```
-BEGIN VERIFY SCRIPT-
git grep -l "push_back(Pair" | xargs sed -i "s/push_back(Pair(\(.*\)));/pushKV(\1);/g"
-END VERIFY SCRIPT-
```
~> Source: https://github.com/bitcoin/bitcoin/commit/91986ed206fa830e5985560c6895b0d30b375054

`perl`:
```
-BEGIN VERIFY SCRIPT-
find src/ -name "*.cpp" ! -wholename "src/util.h" ! -wholename "src/util.cpp" | xargs perl -i -pe 's/(?<!\.)(ParseParameters|ReadConfigFile|IsArgSet|(Soft|Force)?(Get|Set)(|Bool|)Arg(s)?)\(/gArgs.\1(/g'
-END VERIFY SCRIPT-
```
~> Source: https://github.com/bitcoin/bitcoin/commit/bb81e173


(Start your commit message with `scripted-diff: X`, and then on the second line add the `-BEGIN VERIFY SCRIPT-` line. The following lines should contain runnable unix script(s), and finally `-END VERIFY SCRIPT-`.)
