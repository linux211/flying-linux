```
#!/bin/bash
set -o errexit
node_version=8.5.0
#download latest version of nodejs at https://nodejs.org/en/download/ (source codea)
if [ x$1 == x'source' ];then
curl https://nodejs.org/dist/v${node_version}/node-v${node_version}.tar.gz -O
tar -xf node-v${node_version}.tar.gz
cd node-v${node_version}/
./configure
make
make install
/usr/local/bin/npm  install -g gitbook-cli
gitbook -V # wait for installing the latest version of gitbook
elif [ x$1 == x'binary' ];then
curl https://nodejs.org/dist/v${node_version}/node-v${node_version}-linux-x64.tar.xz -O
# install nodejs
tar -xf node-v${node_version}-linux-x64.tar.xz -C /usr/local
ln -sf /usr/local/node-v${node_version}-linux-x64/bin/node /usr/local/bin/node
ln -sf /usr/local/node-v${node_version}-linux-x64/bin/npm /usr/local/bin/npm
# change installation source 
cd /usr/local/node-v${node_version}-linux-x64/lib/node_modules/npm/doc/files 
echo 'registry = http://registry.nmp.taobao.org' >>  npmrc.md
# install gitbook
npm install -g gitbook-cli
cd /usr/local/node-v${node_version}-linux-x64/lib/node_modules/gitbook-cli/node_modules/npmi/node_modules/npm/doc/files
echo 'registry = http://registry.nmp.taobao.org' >>  npmrc.md
ln -sf /usr/local/node-v${node_version}-linux-x64/bin/gitbook /usr/local/bin/gitbook
else
echo Usage: "sh $(basename $0) [source|binary]"
exit 
fi
# check gitbook version
gitbook -V

```
