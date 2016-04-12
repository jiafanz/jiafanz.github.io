---
layout: page
permalink: /research/
title: Research
pubs:

    - title:   "Approach for Urban Popular Event Detection Using Mobile Crowdsourcing Data"
      author:  "ZHANG Jia-fan;GUO Bin;LU Xin-jiang;YU Zhi-wen;ZHOU Xing-she"
      journal: "Transactions on Black Magic"
      note:    "(presented at Oz)"
      year:    "2016"
      url:     "http://publish-more-stuff.org"
      doi:     "http://dx.doi.org"

    - title:   "Trending Words Based Event Detection in Sina Weibo"
      author:  "Lu, Xinjiang and Yu, Zhiwen and Guo, Bin and Zhang, Jiafan and Chin, Alvin and Tian, Jilei and Cao, Yang"
      journal: "Transactions on Black Magic"
      note:    "(presented at Oz)"
      year:    "2015"
      url:     "http://publish-more-stuff.org"
      doi:     "http://dx.doi.org"

---

## Publications

{% for pub in page.pubs %}
{% if pub.internal %}[{{pub.title}}]({{pub.url | prepend: site.baseurl}}){% else %}[{{pub.title}}]({{pub.url}}){% endif %}<br />
{{pub.author}}<br />
*{{pub.journal}}*
{% if pub.note %} *({{pub.note}})*
{% endif %} *{{pub.year}}* [(doi)]({{pub.doi}})
{% endfor %}