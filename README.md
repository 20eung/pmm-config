# pmm-config
Plex Meta Manager Configuration

# Tree

```
/data/pmm/
├── config
│   ├── UUID
│   ├── assets
│   ├── config.all.yml
│   ├── config.movie.foreign.yml
│   ├── config.movie.kor.yml
│   ├── config.movie.new-clear.yml
│   ├── config.movie.new.yml
│   ├── logs
│   └── overlays
├── docker-compose.yml
├── pmm-run-movie-foreign.sh
├── pmm-run-movie-kor.sh
├── pmm-run-movie-new-clear.sh
├── pmm-run-movie-new.sh
└── pmm-run.sh
```

# docker-compose.yml

```
version: "2.1"
services:
  plex-meta-manager:
    image: meisnate12/plex-meta-manager:latest
    container_name: plex-meta-manager
    hostname: pmm
    restart: unless-stopped
    network_mode: host
    environment:
      - TZ=Asia/Seoul
      - PLEX_MEDIA_SERVER_USER=plex
      - PUID=1002
      - PGID=1003
    volumes:
      - /etc/timezone:/etc/timezone
      - ./config:/config
```

PUID, PGID는 plex-media-server 실행할 ID의 것을 사용합니다.

리눅스 쉘에서 id 입력하면 uid와 gid를 알아낼 수 있습니다.

```
ubuntu@:/data$ id
uid=1001(ubuntu) gid=1001(ubuntu) 
groups=1001(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),119(netdev),120(lxd),999(docker)
```

# config.movie.all.yml

> 설명:

- config 예제 : https://metamanager.wiki/en/latest/defaults/guide.html

- 1.1 영화(최근추가) : Plex에서 생성한 라이브러리명과 동일하게 사용합니다. 띄어쓰기까지 동일하게

- plex url/token : Plex 서버를 웹으로 접속한 후 확인할 수 있습니다.

  - 영상을 선택한 후 ... 눌러 미디어 정보를 클릭한 후 XML 보기를 누르면 새 창이 나옵니다.

  - 앞 부분이 URL이고, 마지막 부분이 Token 입니다.

- tmdb apikey : tmdb 회원 가입 후 새성할 수 있습니다.


```
## This file is a template remove the .template to use the file

libraries:
  1.1 영화(최근추가):
    overlay_path:
    - remove_overlays: false
    - reapply_overlay: true
    - pmm: commonsense
    - pmm: flixpatrol
      template_variables:
        position: left
        time_window: this_year
    - pmm: ratings
      template_variables:
        rating1: user
        rating1_image: rt_tomato

        rating2: critic
        rating2_image: imdb

        rating3: audience
        rating3_image: tmdb

        horizontal_position: right

    - pmm: resolution
    - pmm: ribbon
    - pmm: streaming
    settings:
      asset_directory:
      - config/assets
    operations:
#     mass_user_rating_update: mdb_tomatoes
      mass_critic_rating_update: imdb
      mass_audience_rating_update: tmdb
      mass_genre_update: tmdb
#     mass_content_rating_update: mdb_commonsense
      mass_originally_available_update: tmdb
      mass_imdb_parental_labels: without_none
      
  1.2 영화(외국):
    overlay_path:
    - remove_overlays: false
    - reapply_overlay: true
    - pmm: commonsense
    - pmm: flixpatrol
      template_variables:
        position: left
        time_window: this_year
    - pmm: ratings
      template_variables:
        rating1: user
        rating1_image: rt_tomato

        rating2: critic
        rating2_image: imdb

        rating3: audience
        rating3_image: tmdb

        horizontal_position: right

    - pmm: resolution
    - pmm: ribbon
    - pmm: streaming
    settings:
      asset_directory:
      - config/assets
    operations:
      mass_user_rating_update: mdb_tomatoes
      mass_critic_rating_update: imdb
      mass_audience_rating_update: tmdb
      mass_genre_update: tmdb
      mass_content_rating_update: mdb_commonsense
      mass_originally_available_update: tmdb
      mass_imdb_parental_labels: without_none

settings:
  cache: true                           # true or false
  cache_expiration: 60                  # 60 or any integer
  asset_directory:                      # any directory
  asset_folders: true                   # true or false
  asset_depth: 0                        # 0 or any integer
  create_asset_folders: false           # false or true
  prioritize_assets: false              # false or true
  dimensional_asset_rename: false       # false or true
  download_url_assets: false            # false or true
  show_missing_season_assets: false     # false or true
  show_missing_episode_assets: false    # false or true
  show_asset_not_needed: true           # true or false
  sync_mode: append                     # append or sync
  default_collection_order: release     # release or alpha or custom
  minimum_items: 1                      # 1 or any integer
  item_refresh_delay: 0                 # 0 or any integer
  delete_below_minimum: false           # false or true
  delete_not_scheduled: false           # false or true
  run_again_delay: 0                    # 0 or any integer
  missing_only_released: false          # false or true
  only_filter_missing: false            # false or true
  show_unmanaged: true                  # true or false
  show_filtered: false                  # false or true
  show_options: true                    # false or true
  show_missing: true                    # true or false
  show_missing_assets: true             # true or false
  save_report: true                     # false or true
  tvdb_language: kor                    # Any ISO 639-2 Language Code
  ignore_ids:                           # List or comma-separated string of TMDb/TVDb IDs
  ignore_imdb_ids:                      # List or comma-separated string of IMDb IDs
  playlist_sync_to_user: all            # all, list of users, or comma-separated string of users
  playlist_report: false                # false or true
  verify_ssl: true                      # true or false
  custom_repo:                          # link to base repository
  check_nightly: true                   # true or false
  show_unconfigured: true


## Required
  playlist_exclude_users:
plex:
  url: https://34-64-1~~~.plex.direct:32400
  token: Ek4PJGxH~~~
  timeout: 60                           # 60 or any integer
  clean_bundles: false                  # false or true
  empty_trash: false                    # false or true
  optimize: false                       # false or true


## Required
tmdb:                                   # REQUIRED for the script to run
  apikey: 9cea8d49d95
  language: ko                          # ISO 639-1 Code of the User Language
  region: KR                            # ISO 3166-1 Code of the User Region for Searches
  cache_expiration: 60                  # Number of days

```

# pmm-run.sh

pmm 은 지정된 시간에 매일같이 라이브러리 정보를 업데이트합니다.

수동으로 하기 위한 스크립트입니다.

```
#!/bin/sh

docker run --rm -it -v "./config:/config:rw" meisnate12/plex-meta-manager --config /config/config.movie.all.yml --ignore-schedules --run
```
