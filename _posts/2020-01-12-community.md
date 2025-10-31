---
layout: post
title:  "社区和交流"
author: sal
categories: [ 社区 ]
image: assets/images/3.jpg
---

<div class="community-container">
  <h2>欢迎加入我们的机械臂社区!</h2>
  
  <section class="forum-section">
    <h3>群论坛</h3>
    <p>在我们的群论坛上与其他用户交流经验、分享想法、寻求帮助。</p>
    <img src="{{site.baseurl}}/assets/images/qr_group.jpg" alt="群论坛二维码" class="qr-image">
  </section>

  <section class="social-media">
    <h3>社交媒体</h3>
    <p>关注我们的社交媒体账号,获取最新资讯:</p>
    <ul>
      <li><a href="{{site.social.xiaohongshu}}" target="_blank">小红书</a></li>
      <li><a href="{{site.social.bilibili}}" target="_blank">Bilibili</a></li>
    </ul>
  </section>

  <section class="feedback">
  <h3>反馈与建议</h3>
  <p>我们重视您的意见!请填写以下表单向我们提供反馈。</p>
  <form action="mailto:your-email@example.com" method="post" enctype="text/plain">
    <label for="name">姓名:</label>
    <input type="text" id="name" name="name" required>

    <label for="email">邮箱:</label>
    <input type="email" id="email" name="email" required>

    <label for="message">留言:</label>
    <textarea id="message" name="message" required></textarea>

    <button type="submit">提交</button>
  </form>
</section>

<style>
  .community-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  .community-container h2 {
    text-align: center;
    color: #333;
  }
  .community-container section {
    margin-bottom: 30px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 5px;
  }
  .community-container h3 {
    color: #2c3e50;
  }
  .btn {
    display: inline-block;
    padding: 10px 20px;
    background-color: #3498db;
    color: white;
    text-decoration: none;
    border-radius: 5px;
  }
  .qr-image {
    display: block;
    max-width: 300px;
    width: 100%;
    height: auto;
    margin: 20px auto;
    border-radius: 5px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }
  form {
    display: flex;
    flex-direction: column;
  }
  form input, form textarea {
    margin-bottom: 10px;
    padding: 5px;
  }
  form button {
    padding: 10px;
    background-color: #2ecc71;
    color: white;
    border: none;
    cursor: pointer;
  }
</style>