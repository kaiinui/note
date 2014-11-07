Appledoc
---

```sh
#!/bin/bash

appledoc --prefix-merged-sections \
 --ignore ".m"\
 --ignore "Pods"\
 --keep-undocumented-objects\
 --keep-undocumented-members\
 --create-html\
 --no-create-docset\
 --include "docs/_doc_flow.png"\
 --index-desc "docs/_doc_index.markdown"\
 --company-id "org.openaquamarine"\
 --project-name="AquaSync"\
 --project-company "Aquamarine"\
 --output "~/dropbox/Public/___doc___AquaSync"\
 AquaSync/Classes
```
