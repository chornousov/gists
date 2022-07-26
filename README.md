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
EOF
```

### git commit short
```bash
git rev-parse --short HEAD
# OR
git log -1 --pretty=%H | cut -c-7
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