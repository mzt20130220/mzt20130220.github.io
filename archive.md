---
layout: default
---
<main class="main">
    <div class="container">
        <section class="archive-content">
            <article class="post">
                <h2 class="post-title">文章归档</h2>
                
                {% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
                {% for year in posts_by_year %}
                <div class="archive-year">
                    <h3>{{ year.name }}年</h3>
                    <ul class="archive-list">
                        {% for post in year.items %}
                        <li>
                            <span class="archive-date">{{ post.date | date: "%m月%d日" }}</span>
                            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
                        </li>
                        {% endfor %}
                    </ul>
                </div>
                {% endfor %}
            </article>
        </section>

        <aside class="sidebar">
            <div class="widget">
                <h3>热门标签</h3>
                <div class="tag-cloud">
                    {% for tag in site.tags %}
                    <span class="tag">{{ tag[0] }}</span>
                    {% endfor %}
                </div>
            </div>
        </aside>
    </div>
</main>