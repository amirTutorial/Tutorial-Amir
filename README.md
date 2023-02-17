$engine: 3
$onesync: legacy
name: PlumeESXLegacy
version: 3.3.0
author: Tabarra & SaltySea / Edit by Nemesus.de
description: A full featured and highly configurable yet lightweight ESX Legacy base that can be easily extended. 

tasks:
  # Download default CFX resources
  - action: download_github
    src: https://github.com/citizenfx/cfx-server-data
    ref: master
    subpath: resources
    dest: ./resources/[cfx-default]
  
  # Download and prepare server.cfg / loadingscreen / database
  - action: download_github
    src: https://github.com/tabarra/PlumeESX-recipe
    ref: main
    dest: ./tmp/plume_esx

  - action: move_path
    src: ./tmp/plume_esx/server.cfg
    dest: ./server.cfg

  - action: move_path
    src: ./tmp/plume_esx/loadingscreen
    dest: ./resources/loadingscreen

  - action: connect_database
  - action: query_database
    file: ./tmp/plume_esx/plume.sql

  # Download basic resources and ESX dependencies
  - action: download_github
    src: https://github.com/esx-framework/esx-legacy
    ref: 8153f6a9221baeef0affb0176e5811007b8f5e3b
    dest: ./resources/[legacy]

  - action: download_github
    src: https://github.com/tabarra/simpledrift
    ref: 2ed57c224ae70549fbcfccc957a00ff496b78c77
    dest: ./resources/simpledrift

  - action: download_github
    src: https://github.com/Bob74/bob74_ipl
    ref: e949c08c67bdfe3fb981bf4f77477faf26d187e6
    dest: ./resources/bob74_ipl
  
  - action: download_github
    src: https://github.com/brouznouf/fivem-mysql-async
    ref: c5fa317a65acfe2eef453257e19e3b4fde137089
    dest: ./resources/mysql-async

  - action: download_github
    src: https://github.com/AvarianKnight/pma-voice
    ref: 4754a4dfff1e897f14df6d7d0e06c7fbfbaa20d5
    dest: ./resources/pma-voice

  - action: download_github
    src: https://github.com/AvarianKnight/rp-radio
    ref: 558e14d3eb7eed62e780a30c2867eadb61ffe0ac
    dest: ./resources/rp-radio

  - action: download_github
    src: https://github.com/thelindat/linden_outlawalert
    ref: a28b0ab65b1a4a9fed4063a26566cf697c263fc8
    dest: ./resources/shotspotter

  - action: download_github
    src: https://github.com/Blumlaut/EasyAdmin
    ref: 776ac8f3898182e5c075d533bbd89e92efa017e9
    dest: ./resources/EasyAdmin

  - action: download_github
    src: https://github.com/DevTestingPizza/vSync
    ref: 65f0e3cdd5692b619985a7df6297ffca7db41525
    dest: ./tmp/vSync

  - action: move_path
    src: ./tmp/vSync/vSync
    dest: ./resources/vSync

  ## GCPhone stuff
  - action: download_file
    url: https://github.com/Re-Ignited-Development/Re-Ignited-Phone/releases/download/V1.5/resources-1.2.zip
    path: ./tmp/gcphone.zip
  - action: unzip
    src: ./tmp/gcphone.zip
    dest: ./tmp
  - action: move_path
    src: ./tmp/resources-1.2/gcphone
    dest: ./resources/gcphone

  ## vstancer stuff
  - action: download_file
    url: https://github.com/carmineos/fivem-vstancer/releases/download/v1.4/vstancer-v1.4.0.zip
    path: ./tmp/vstancer.zip
  - action: unzip
    src: ./tmp/vstancer.zip
    dest: ./tmp
  - action: move_path
    src: ./tmp/vstancer
    dest: ./resources/vstancer


  ## Downloading & Patching configs: cosmo hud
  - action: download_github
    src: https://github.com/nojdh/cosmo_hud
    ref: eed8fd7b8cbe8a02ae7339d4dee05b5d56a8097c
    subpath: cosmo_hud
    dest: ./resources/[hud]/cosmo_hud

  - action: replace_string
    file: ./resources/shotspotter/config.lua
    search: 'Config.Default911 = false'
    replace: 'Config.Default911 = true'

  - action: replace_string
    file: ./resources/[legacy]/[esx_addons]/esx_status/config.lua
    search: "    Config.Display        = true"
    replace: "    Config.Display        = false"

  - action: replace_string
    file: ./resources/[hud]/cosmo_hud/config.lua
    search: 'Config.AlwaysShowRadar = false'
    replace: 'Config.AlwaysShowRadar = true'

  ## Patching configs: esx_basicneeds
  - action: replace_string
    file: ./resources/[legacy]/[esx_addons]/esx_basicneeds/config.lua
    search: 'Config.Visible = true'
    replace: 'Config.Visible = false'

  ## Patching configs: multicharacter 
  - action: replace_string
    file: ./resources/[legacy]/[esx]/es_extended/config.lua
    search: 'Config.Multichar                = false'
    replace: 'Config.Multichar = true '

  - action: replace_string
    file: ./resources/[legacy]/[esx_addons]/esx_multicharacter/server/main.lua
    search: "Config.Database = 'es_extended'"
    replace: "Config.Database = '{{dbName}}'"


  ## Patching configs: rp-radio
  - action: replace_string
    file: ./resources/rp-radio/config.lua
    search: |
      Name = "INPUT_REPLAY_START_STOP_RECORDING_SECONDARY", -- Control name"
    replace: | 
      Name = "INPUT_SELECT_CHARACTER_MICHAEL", -- Control name"

  - action: replace_string
    file: ./resources/rp-radio/config.lua
    search: 'Key = 289, -- F2'
    replace: 'Key = 166, -- F5'

  - action: replace_string
    mode: all_vars
    file: 
    - ./resources/loadingscreen/config.js
    - ./resources/[legacy]/[esx_addons]/esx_multicharacter/server/main.lua

  
  ## Cleanup
  - action: remove_path
    path: ./tmp
  - action: remove_path
    path: ./resources/esx_example
  - action: remove_path
    path: ./resources/server.cfg
  - action: remove_path
    path: ./resources/[legacy]/esx_example
  - action: remove_path
    path: ./resources/[legacy]/[esx_addons]/esx_whitelist
  - action: remove_path
    path: ./resources/[legacy]/[esx_addons]/esx_voice
  - action: remove_path
    path: ./resources/[legacy]/[esx_addons]/esx_phone
