---
title: 我的说说(如加载失败请刷新)
date: 2023-05-09
reward: true
---

<head>
  <!-- ... -->
  <script src="https://cdn.jsdelivr.net/gh/Uyoahz26/daodao@main/dist/qexo-dao.min.js"></script>
  <!-- ... -->
</head>
<body>
  <!-- ... -->
  <div id="qexoDaoDao"></div>
  <script>
    qexoDaodao?.init({
      el: "#qexoDaoDao",
      avatar: "https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/info/user.png",
      name: "圣奇宝枣",
      title: "博主的碎碎念",
      limit: 7,
      useLoadingImg: false,
      baseURL: "https://bk.shengqibaozao.eu.org",
    }).then(function (){
      console.log("说说加载完成");
    })
  </script>
</body>
