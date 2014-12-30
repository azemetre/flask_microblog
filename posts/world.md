title: The World is Calling
date: 2014-09-07
published: true

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Corporis voluptatum dignissimos dolorum sequi quasi! Voluptatem fugit sed incidunt magnam inventore!

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Recusandae totam explicabo adipisci, itaque cum fugit quasi ad, ratione iusto nobis repellendus eos laboriosam voluptatem? Placeat doloremque, excepturi, molestiae nulla iure, laudantium reiciendis soluta id aliquid beatae inventore repellendus quo laborum consequuntur cumque eaque porro illum dolore et. Modi doloribus magnam nobis deleniti quasi aperiam reprehenderit.

<img src="http://lorempixel.com/400/200/" alt="Placeholder picture">

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odio consectetur expedita, similique mollitia. Recusandae, id?

    :::python
    from urlparse import urljoin
    from flask import request
    from werkzeug.contrib.atom import AtomFeed


    def make_external(url):
        return urljoin(request.url_root, url)


    @app.route('/recent.atom')
    def recent_feed():
        feed = AtomFeed('Recent Articles',
                        feed_url=request.url, url=request.url_root)
        articles = Article.query.order_by(Article.pub_date.desc()) \
                          .limit(15).all()
        for article in articles:
            feed.add(article.title, unicode(article.rendered_text),
                     content_type='html',
                     author=article.author.name,
                     url=make_external(article.url),
                     updated=article.last_update,
                     published=article.published)
        return feed.get_response()
