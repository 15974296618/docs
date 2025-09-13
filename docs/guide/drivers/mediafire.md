---
author: Da3zKi7 (D@' 3z K!7)
# This is the icon of the page
icon: iconfont icon-state
# This control sidebar order
order: 185
# A page can have multiple categories
category:
  - Guide
# A page can have multiple tags
tag:
  - Storage
  - Guide
  - "302"
# this page is sticky in article list
sticky: true
# this page will appear in starred articles
star: true
---

# MediaFire

<br/>

![logo](/img/drivers/mediafire/mediafire_mf_logo_u1_full_color_reversed.svg)

Site：**https://mediafire.com**
<br/>

- MediaFire does not provide `API_KEY` nor `APP` support anymore, so setting user session values is a must.
- :warning: Alist version > ==3.53.0== to get this driver.

## **Configure storage**

1. Go **http://localhost:5244/@manage/storages** or your custom AList web
2. Press "Add" button to bind another storage
3. Choose "MediaFire"
4. Set Mount Path, i.e. /MediaFire/MyCloud
5. Go **https://mediafire.com** in another browser tab
6. Open Dev Tools by pressing F12 or (Ctrl / Command) + Shift + I
7. Press "Network" tab (upper bar)
8. Press F5 to refresh and start intercepting all requests

9. Copy the `Session Token`

   ![session_token](/img/drivers/mediafire/mediafire_session_token.png)

10. Switch tab to AList Admin and Paste it into Session Token field

11. Switch tab to MediaFire and Copy the `Cookie`

    ![cookie](/img/drivers/mediafire/mediafire_cookie.png)

12. Switch back tab to AList Admin and Paste it into Cookie field

13. Verify Session Token and Cookie are set

    ![session_token_cookie](/img/drivers/mediafire/mediafire_session_token_cookie.png)

<br/>

14. Press "Add" button again to confirm your MediaFire storage. Done!

<br/>

## **Root folder ID**

Default is "/", because this driver roots to "myfiles", and then manages directories to folderID like "xxxyyyzzz123".

- Custom folder root is currently not supported since MediaFire dir structure is based in IDs, not in sequential navigation i.e. /myfiles/Photos/Christmas/

<br/>

### **Features**

1. List, Link, MakeDir, Move, Rename, Copy, Remove, Put, PutResult

2. Session auto-refreshing

3. Upload is resumable and supports recovery, due to splitted in chunks transmission. Very useful for big files.

<br/>

### **Tips**

1. `root folder ID`,`root folder Path` will be set automatically

2. MediaFire closes the current session after 10 mins, an auto renewal of session (as stated before) is already implemented, however if you shut down AList or your PC, session will expire and reconfiguring session and cookie is mandatory, in order to keep the storage working!

3. If you have AList running in a server or smartphone, you will likely do not have problems about the uggly session expiry, unless AList server get off.

<br/>

### **The default download method used**

```mermaid
---
title: Which download method is used by default?
---
flowchart TB
    style a1 fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff
    style a2 fill:#ff7575,stroke:#333,stroke-width:4px
    subgraph ide1 [ ]
    a1
    end
    a1[302]:::someclass====|default|a2[user equipment]
    classDef someclass fill:#f96
    c1[local proxy]-.alternative.->a2[user equipment]
    b1[Download proxy URL]-.alternative.->a2[user equipment]
    click a1 "../drivers/common.html#webdav-policy"
    click b1 "../drivers/common.html#webdav-policy"
    click c1 "../drivers/common.html#webdav-policy"
```
