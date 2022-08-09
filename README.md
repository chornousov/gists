### docker: Delete dangling images
```bash
if [[ $(docker images -f "dangling=true" -q) ]]; then
    docker rmi $(docker images -f "dangling=true" -q)
fi
```

### Bash to upper
```bash
upper=$(echo ${lower^^})
```

### docker: npm install with cache
```bash
docker run --rm \
    -v $(pwd):/app \
    -v /path-to-host-cache-folder:/root/.npm \
    -w /app \
    sh -c "npm ci --prefer-offline --no-audit"
```

### reload bashrc / apply bashrc
```bash
source ~/.bashrc
# OR
. ~/.bashrc
```

### grep highlight match / grep show beta dependencies
```bash
cat package.json | jq -r '.dependencies' | grep --color=always "beta\|$"
# `jq -r` removes double-quotes around the output
```

### grep filter lines and highlight match
```bash
cat package.json | jq -r '.dependencies' | grep --color=always "beta"
# `jq -r` removes double-quotes around the output
```

### grep check if any results
```bash
cat package.json | grep "nuxt"
rc=$?
if [[ ${rc} -eq 0 ]]; then
    echo 'has matches'
else
    echo 'no match'
fi
# OR
if cat package.json | grep -q "nuxt"; then
    echo 'has matches'
else
    echo 'no match'
fi
```

### bash check date
```bash
lastDayStr='2022-08-17'
lastDay=$(date -d $lastDayStr +%s)
today=$(date +%s)
if [ $today -gt $lastDay ]; then
# if (( $today > $lastDay )); then
    echo 'expired'
else 
    echo 'ok'
fi
``` 

### bash compare / bash if greater / bash if compare
```bash
if [ $a -gt $b ]; then
if (( $a > $b )); then
```

### bash folder exist / bash file exist / bash files by extension exist
```bash
if [ -d 'foldername' ]  # folder exist
[ -d 'folder' ] && echo 'folder exist oneliner'
if [ ! -d 'folder' ]    # folder does not exist

```

### bash get package version from package.json 
```bash
versionRegex='".?[0-9]+\.[0-9]+\.[0-9]+"'
version=$(cat package.json | 
    grep -E '"nuxt": '$versionRegex | # grep the line with package version
    grep -oE $versionRegex | # extract the version number
    sed 's/"//g' # remove double quotes
)
```

### bash if string is in the list
```bash
list="14 | 16"
if [[ $imageTag =~ $list ]]
```

### grep or
```bash
cat package.json | grep -E '"(tslint|eslint)": "'
```

### bash create file / bash create multiline file
```bash
cat > filename.txt << EOF
echo 'content goes here'
# if you want to access variables in runtime instead of 'creation' time, need to escape $ sign
# \$runtime_variable
# $creationtime_variable
EOF
```

### git commit short / git sha
```bash
git rev-parse --short HEAD
# OR
git log -1 --pretty=%H | cut -c-7
# OR
git show -s --format='%H'  # long sha
git show -s --format='%h'  # short
```

### git author git email
```bash
git show -s --format='%aE' # email
git show -s --format='%aN' # name
```

### bash substring
```bash
str="some string"
echo ${str:0:3}
```

### bash remove line / delete line from file
```bash
sed -i '/text/d' ./filename
```

### powershell filter / powershell where 
if the result of the filter is a single item - powershell treats it as an object, not array. Need to explicitly make it array
```powershell
([Array]($list | where prop -eq 'something'))
```

### powershell select multiple fields
```powershell
$list | select-object -property Prop1,Prop2
```

### azure devops powershell / azuredevops powershell
In order to not introduce unwanted Task group input parameters:
```powershell
Write-Host ("{0}" -f $variable)
# instead of 
Write-Host "$variable"

# AND

$env:TempPipelineVariable
# instead of 
$(TempPipelineVariable)
```

### powershell rest 
```powershell
$result=Invoke-RestMethod -Method GET `
    -Uri "uri" `
    -Headers @{'accept'='stuff'}
```

### bash curl
```bash
result=$(curl -s --request POST \
    --url 'url' \
    --header 'accept: stuff')
```

### jq select 
```bash
# { items: [ {name: 'smth'} ] }
result=$(echo $jsonString | jq '.items[] | select(.name=="'"$name"'")')
```

### js ignore first items in array / skip first array items
```js
const [ , , ...args] = process.argv
// OR
const args = process.argv.splice(2)
```

### bash -c multiline / sh -c multiline
```bash
docker run --rm node:16.13.1-alpine sh -c "
    node -v &&
    npm -v &&
    echo 'something' &&
    printenv"
```

### bash get column / docker images id
```bash
# filter by name, take 3rd column - imageID
imageID=$(docker images | grep yourimagename | awk '{print $3}')
```

### bash variable empty / bash variable not set
```bash
if [ -z "$variable" ]; then
    echo 'empty'
else 
    echo 'has value'
fi
```

### bash remove files by user
```bash
find . -user root | xargs rm -rf
```

### curl performance metrics
```bash
curl -L -w "
   time_namelookup: %{time_namelookup}s
      time_connect: %{time_connect}s
   time_appconnect: %{time_appconnect}s
  time_pretransfer: %{time_pretransfer}s
     time_redirect: %{time_redirect}s
time_starttransfer: %{time_starttransfer}s
 ----------
        time_total: %{time_total}s\n" -o /dev/null -s https://google.com
```
OR
```bash
curl -L -w "\n   time_namelookup: %{time_namelookup}s\n      time_connect: %{time_connect}s\n   time_appconnect: %{time_appconnect}s\n  time_pretransfer: %{time_pretransfer}s\n     time_redirect: %{time_redirect}s\ntime_starttransfer: %{time_starttransfer}s\n
 ----------\n        time_total: %{time_total}s\n" -o /dev/null -s https://google.com
```
OR 
```bash
echo "
   time_namelookup: %{time_namelookup}s\n
      time_connect: %{time_connect}s\n
   time_appconnect: %{time_appconnect}s\n
  time_pretransfer: %{time_pretransfer}s\n
     time_redirect: %{time_redirect}s\n
time_starttransfer: %{time_starttransfer}s\n
 ----------\n
        time_total: %{time_total}s\n" > curl_metrics.txt

curl -L -w "@curl_metrics.txt" -o /dev/null -s https://google.com
```

### bash verify files in folder
```bash
if test -n "$(find . -type f -name "*.spec.ts" -print -quit)"; then
    echo 'Found files'
fi
```

### linux server info / os version / kernel
``` bash
cat /etc/os-release
uname -a
```

### linux server info full / linux stats
```bash
cat /etc/os-release
uname -a
# busybox version
# busybox | head -1
free -mt    # memory in MB + total
# egrep 'Mem|Cache|Swap' /proc/meminfo
df -h       # disk space

printenv

docker -v
docker images

node -v && npm -v
npm list -g --depth 0   # npm global packages installed 

dotnet --info
# dotnet --version
# dotnet --list-sdks
# dotnet --list-runtimes
```

### bash folder size
```bash
du -hs      # current dir
du -hs *    # all in current dir
du -h       # ALL RECURSIVE
du -hs * | sort -h -r # sort by size
```

### bash env variables
```bash
export PATH=/some/path:$PATH
printenv
```

### grep result / grep empty
```bash
if printenv | grep -q "dotnet"; then echo 'exist'; else echo 'nope'; fi
```

### cypress git info docker
```yaml
environment:
- COMMIT_INFO_BRANCH=$(Build.SourceBranch)
- COMMIT_INFO_MESSAGE=$(Build.SourceVersionMessage)
- COMMIT_INFO_EMAIL=$(git show -s --format='%aE')
- COMMIT_INFO_AUTHOR=$(git show -s --format='%aN')
- COMMIT_INFO_SHA=$(git show --pretty=format:'%H')
- COMMIT_INFO_REMOTE=$(git config --get remote.origin.url)
```