---
date: "2025-03-13T15:24:57+09:00"
draft: false
title: Step By Step Using This Template
type: post
math: true
---
## Installing gohugo

To install gohugo, please follow this [link](https://gohugo.io/installation/).

## Create new hugo project
```bash
hugo new site dtnou.gitlab.io
```

Create new Hugo Site.
## Clone the themes
```git
git submodule add https://gitlab.com/dtn-ou/dtnou-hugo.git themes/dtnou-hugo
```

if error :
```bash
The following paths are ignored by one of your .gitignore files:
themes
themes/dtnou-hugo
hint: Use -f if you really want to add them.
hint: Disable this message with "git config set advice.addIgnoredFile false"
```
add force (-f)
```git
git submodule add -f https://gitlab.com/dtn-ou/dtnou-hugo.git themes/dtnou-hugo
```

## hugo.yaml Configuration file
This gohugo using ```hugo.yaml```, the config file can be toml and json. This full example of the ```hugo.yaml```.

```yaml
baseURL: 'https://example.org/'
languageCode: 'en-us'
title: 'My New Hugo Site'
theme: 'dtnou-hugo'

markup:
  highlight:
    style: friendly

menus:
  main:
    - name: CV
      pageRef: /cv
      weight: 10
    - name: Publications
      pageRef: /publications
      weight: 20
    - name: Posts
      pageRef: /posts/
      weight: 30
    - name: Use This template
      pageRef: /posts/howto
      weight: 40


params:
  author:
    name: 'DTN OU'
    location: 'Japan'
    #pronouns:
    bio: "PhD Students at Okayama University"
    employer: 'Okayama University'
    #picture: 'profile.png'
    site: 'https://dtn-ou.gitlab.io'
    mail: 'mpranata@s.okayama-u.ac.jp'
    # Full Link
    google_scholar: 'https://scholar.google.com/citations?user=000000000000&hl=en'
    scopus: 'https://www.scopus.com/authid/detail.uri?authorId=00000000000'
    #arxiv:
    #orcid:
    #semantic:
    #impactstory:
    #pubmed:
    #researchgate:
    #mastodon:
    #medium:

    # Username
    #bitbucket:
    #codepen:
    #dribbble:
    #github: 'mgpnata'
    gitlab: 'dtn-ou'
    #kaggle:
    #stackoverflow:
    #bluesky:
    #facebook:
    #flickr:
    #foursquare:
    #goodreads:
    #google_plus:
    #keybase:
    #instagram:
    #lastfm:
    #linkedin:
    #pinterest:
    #soundcloud:
    #steam:
    #telegram:
    #tumblr:
    #twitter:
    #vine:
    #weibo:
    #wikipedia:
    #xing:
    #youtube:
    #zhihu:

```
### Change Photo
To change photo profile, put the photo on the ```static/images/``` directory, and write the name of the file on the ```picture``` on the configuration file.
## Setup the home page
The home page of this theme used the ```_index.md``` on the content directory. Edit the file, and put your information.

```md
---
date: "2025-03-11T12:00:00+09:00"
draft: false
title: DTN-OU
---
[DTN-OU](https://gitlab.com/dtn-ou/dtn7) is fork from [DTN7](https://dtn7.github.io), with focus on securing the transmission on the DTN7-Go. For the first implementation, we use AES-128 bit for securing the bundle payload. The key of each node saved on the configuration file.

## Step by Step using this template
To use this template, follow [this link]({{<ref "posts/howto.md">}}).
```

## Setup the CV page
The CV page using standard Markdown. Create ```cv.md``` on the content directory, put ```url: cv``` and ```layout: cv``` as params.
```md
---
title: CV / About Me
url: cv
layout: cv
draft: false
---
# Academics
- Phd Students at Okayama Unviersity
- Master at Master University
- Bsc at Bsc University

# Work experience
- xxx
  - yyy
  - zzz
- xx1
  - yy1

# Skills
- Skill 1
- Skill 2
  - Sub Skill
```

## Setup the Publications page
This publications page use specific format. To make it easy, just copy the bibtex and make small change to use the format. Create ```publications.md``` on the content directory.
```md
---
title: Publications
url: publications
layout: publications
draft: false
journals:
  -
    title: 'All about Delay-Tolerant Networking (DTN) Contributions to Future Internet'
    author:
      - '**Author 1**'
      - 'Author 2'
      - 'Author 3'
    journal: 'Future Internet'
    volume: 16
    issue: 4
    pages: 129
    year: 2024
    doi: 'https://doi.org'
    link: 'https://www.mdpi.com/1999-5903/16/4/129'

conferences:
  -
    title: 'DTN7: An Open-Source Disruption-Tolerant Networking Implementation of Bundle Protocol 7'
    author:
      - '**Author 1**'
      - 'Author 2'
      - 'Author 3'
    conference: '18th International Conference on Ad-Hoc Networks and Wireless, ADHOC-NOW 2019'
    location: 'Luxembourg,Luxembourg'
    pages: '196-209'
    year: '2019'
    doi: 'https://doi.org'
    link: 'https://link.springer.com/10.1007/978-3-030-31831-4_14'

domesticjournals:
  -
    title: 'Title Journal 2'
    author:
      - '**Author 1**'
    journal: 'Journal'
    volume:
    issue:
    pages:
    year:
    doi: 'https://doi.org'
    link: 'doi'

domesticconferences:
  -
    title: 'Artikel Jurnal 1'
    author:
      - '**Author 1**'
    conference: 'Journal'
    location: ''
    pages: ''
    year: ''
    doi: 'https://doi.org'
    link: 'doi'
---
```
## Deploy to GitLab Pages / Github Pages
To Deploy on GitLab, follow [this](https://docs.gitlab.com/tutorials/hugo/) or [this](https://gohugo.io/host-and-deploy/host-on-gitlab-pages/). To Deploy on GitHub, follow [this](https://gohugo.io/host-and-deploy/host-on-github-pages/).

### .gitlab-ci.yml


```gitlab
default:
  image: "${CI_TEMPLATE_REGISTRY_HOST}/pages/hugo/hugo_extended:latest"

variables:
  GIT_SUBMODULE_STRATEGY: recursive

test:  # builds and tests your site
  script:
    - git submodule update --recursive --remote
    - hugo
  rules:
    - if: $CI_COMMIT_BRANCH != $CI_DEFAULT_BRANCH

deploy-pages:  # a user-defined job that builds your pages and saves them to the specified path.
  script:
    - git submodule update --recursive --remote
    - hugo
  pages: true  # specifies that this is a Pages job
  artifacts:
    paths:
      - public
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
  environment: production
```
