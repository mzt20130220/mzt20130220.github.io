---
layout: default
---
<main class="main">
    <div class="container">
        <section class="about-content">
            <article class="post">
                <h2 class="post-title">关于我</h2>
                <div class="about-text">
                    <p>欢迎来到我的技术博客！我是一名热爱编程的前端开发者，专注于 Web 技术领域。</p>
                    
                    <h3>个人简介</h3>
                    <p>拥有多年前端开发经验，对 JavaScript、React、TypeScript 等技术有深入研究。喜欢探索新技术，乐于分享自己的学习心得和实践经验。</p>
                    
                    <h3>技术栈</h3>
                    <ul>
                        <li><strong>前端框架：</strong>React、Vue、Next.js</li>
                        <li><strong>编程语言：</strong>JavaScript、TypeScript</li>
                        <li><strong>样式：</strong>CSS3、SCSS、Tailwind CSS</li>
                        <li><strong>构建工具：</strong>Webpack、Vite</li>
                        <li><strong>后端：</strong>Node.js、Express</li>
                    </ul>
                    
                    <h3>联系方式</h3>
                    <p>如果你有任何问题或建议，欢迎通过以下方式联系我：</p>
                    <ul>
                        <li>GitHub: <a href="https://github.com/{{ site.github_username }}" target="_blank">github.com/{{ site.github_username }}</a></li>
                        <li>Email: {{ site.email }}</li>
                    </ul>
                </div>
            </article>
        </section>

        <aside class="sidebar">
            <div class="widget">
                <h3>最新文章</h3>
                <ul class="recent-posts">
                    {% for post in site.posts limit:5 %}
                    <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
                    {% endfor %}
                </ul>
            </div>
        </aside>
    </div>
</main>