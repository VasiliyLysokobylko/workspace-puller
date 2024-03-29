# Workspace Puller

**A cli tool for building your python workspace and upload results to the Google Drive or Kaggle.com**

## Description

This tools allows to:  
 * download workspace files or archives and extract(bztar, gztar, tar, xztar, zip) it if needed.
 * clone git repos
 * execute a python files
 * upload result to the Google Drive
 * upload result to the Kaggle.com

## Installation:

`pip install workspace-puller`

## Usage

`workspace-puller --config_url {url to the config file} --bot_token {telegram bot token}`

**config_url** parameter specifies the URL to the configuration file (Required). Enclose the value in quotes (Recomended).

**bot_token** parameter sets the Telegram bot token (Optional). [See: Creating a new bot](https://core.telegram.org/bots#6-botfather)

#### Authorization

The `workspace-puller` has two Google Drive authorization methods:

* local CLI. Just add [Google Drive folders IDs](#Google-drive-uploading) in config. .

* remote via telegram chat bot. You need to add [Google Drive folders IDs](#Google-drive-uploading) in config, [telegram chat IDs](#Telegram-channels) and specify CLI param `--bot_token`.
The Telegram bot will send a authorization link, the user must visit link and send the Google token in the response message.

Kaggle credentials should be set inside configuration file in the [Kaggle](#Kaggle-uploading) settings section.

After successful authorization Workspace Puller saves credentials inside workspace.

## Configuartion

Configuration file has a YAML format. The file must be accessible through HTTP. 

#### Dataset list

List of files to download. 
Bztar, gztar, tar, xztar, zip files are unzipped by default. If it is not necessary, specify the node `extract_archives:` with a value `false`. (Default value: true)

*Example:*
```
dataset_list:
  - https://tlk.s3.yandex.net/dataset/TlkAgg2.zip
  - https://tlk.s3.yandex.net/dataset/TlkPersonaChatRus.zip
  - https://tlk.s3.yandex.net/dataset/TlkAgg5.zip
  - https://tlk.s3.yandex.net/dataset/LRWC.zip
  - https://tlk.s3.yandex.net/dataset/WordSenseRus.zip
extract_archives: false    
```

#### Git repos

Repos list for cloning. The repository will clone into a folder with the name specified as the record key.
To add a repository, add a record key with a child node `url: {repo url}`. 

If you need a specific branch, specify its name in the child node `branch:` (Optional).

*Example:*

```
repos:
  downloader: 
    url: https://github.com/bantonj/downloader
  pyreadstat: 
    url: https://github.com/Roche/pyreadstat
    branch: dev
```

#### Executing python code

To run the code, specify a list of files in the root node `script_file:` 

*Example:*
```
script_file:
  - example_file.py
```

#### Output folders

List of folders to upload.

*Example:*
```
output_folder:
  - ./results
  - ./example
```

#### Google drive uploading

List of google drive folder IDs to load results into.

*Example:*
```
google_drive:
  - 1MOUoYor2N2TEbJFDKg-4HDKqF-a9AKRq
  - 1MmhIYor2N2CVRJFDKg-4QACKF-a9AUGq
  - 1ATAGart2N2TEbJFDKg-4QfACN-a9NRGq
```

#### Kaggle uploading

Kaggle new dataset settings

- `username:` Kaggle user name (Required)
- `key:` Kaggel api token (Required). See: https://github.com/Kaggle/kaggle-api#api-credentials
- `slug:` The URL slug of the dataset you want to create. (Default value: `unknown_slug`)
- `license:` 
    You can specify the following licenses for your dataset (Optional. Default value: `CC0-1.0`):

    * `CC0-1.0:` CC0 Public Domain
    * `CC-BY-SA-3.0:` CC BY-SA 3.0
    * `CC-BY-SA-4.0:` CC BY-SA 4.0
    * `CC-BY-NC-SA-4.0:` CC BY-NC-SA 4.0
    * `GPL-2.0:` GPL 2
    * `ODbL-1.0:` Database: Open Database, Contents: © Original Authors
    * `DbCL-1.0:` Database: Open Database, Contents: Database Contents
    * `copyright-authors:` Data files © Original Authors
    * `other:` Other (specified in description)
    * `unknown:` Unknown

- `dataset_title:` Dataset title (Default value: `Unknown title`)
- `isPrivate:` Create publicly (Optional. Default value: `true`)
- `convertToCsv:` Convert tabular files to CSV  (Optional. Default value: `true`)
- `dirmode:` What to do with directories: "skip" - ignore; "zip" - compressed upload; "tar" - uncompressed upload (Optional. Default value: `skip`)

*Example:*

```
kaggle:
  username: user_name
  key: api_key
  slug: slg_value  
  license: CC0-1.0
  dataset_title: dataset_title
  isPrivate: true
  convertToCsv: true
  dirmode: tar
```

#### Telegram channels

Telegram chats list for remote Google Drive authorization. The tool sends a message to these chats after the end of work.

*Example:*

```
telegram_channels:
  - 750423741
  - 750423742
  - 750423743
``` 

### Config example:

```
dataset_list:
  - https://tlk.s3.yandex.net/dataset/TlkAgg2.zip
  - https://tlk.s3.yandex.net/dataset/TlkPersonaChatRus.zip
  - https://tlk.s3.yandex.net/dataset/TlkAgg5.zip
  - https://tlk.s3.yandex.net/dataset/LRWC.zip
  - https://tlk.s3.yandex.net/dataset/WordSenseRus.zip
extract_archives: false  
repos:
  downloader: 
    url: https://github.com/bantonj/downloader
  pyreadstat: 
    url: https://github.com/Roche/pyreadstat
    branch: dev
script_file:
  - file.py
output_folder:
  - ./results 
google_drive:
  - NTHDIXBMWNCGFIXBM-NTbmTHD-TGFIT
kaggle:
  username: OEKOIXBT
  key: THDIDHTR4209YDHTR4209ydhcDHT
  slug: wp_test_slg  
  license: CC0-1.0
  dataset_title: wsp_test_dataset
  isPrivate: true
  convertToCsv: true
  dirmode: tar
telegram_channels:
  - 750423741
```