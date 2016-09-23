Модуль поиска на основе Zend Lucene
===================================

Использование:

- Открыть файл /protected/config/modules/zendsearch.php
- В нем есть описание переменной searchModels:

<pre><code class="php">
<?php

return [
    'module' => [
        'class' => 'application.modules.zendsearch.ZendSearchModule',
        'searchModels' => [
            'News' => [
                'path' => 'application.modules.news.models.News',
                'module' => 'news',
                'titleColumn' => 'title',
                'linkColumn' => 'slug',
                'linkPattern' => '/news/news/view?slug={slug}',
                'textColumns' => 'full_text,short_text,keywords,description,slug',
                'criteria' => [
                    'condition' => 'status = :status',
                    'params' => [
                        ':status' => 1
                    ],
                ],
            ],
            'Page' => [
                'path' => 'application.modules.page.models.Page',
                'module' => 'page',
                'titleColumn' => 'title',
                'linkColumn' => 'slug',
                'linkPattern' => '/page/page/view?slug={slug}',
                'textColumns' => 'body,title_short,keywords,description,slug',
                'criteria' => [
                    'condition' => 'status = :status',
                    'params' => [
                        ':status' => 1
                    ],
                ],
            ],
            'Blog' => [
                'path' => 'application.modules.blog.models.Blog',
                'module' => 'blog',
                'titleColumn' => 'name',
                'linkColumn' => 'slug',
                'linkPattern' => '/blog/blog/view?slug={slug}',
                'textColumns' => 'name,description,slug',
                'criteria' => [
                    'condition' => 'status = :status',
                    'params' => [
                        ':status' => 1
                    ],
                ],
            ],
            'Post' => [
                'path' => 'application.modules.blog.models.Post',
                'module' => 'blog',
                'titleColumn' => 'title',
                'linkColumn' => 'slug',
                'linkPattern' => '/blog/post/view?slug={slug}',
                'textColumns' => 'title,quote,content,slug',
                'criteria' => [
                    'condition' => 'status = :status',
                    'params' => [
                        ':status' => 1
                    ],
                ],
            ],
            'Product' => [
                'path' => 'application.modules.store.models.Product',
                'module' => 'store',
                'titleColumn' => 'name',
                'linkColumn' => 'slug',
                'linkPattern' => '/store/product/view?slug={slug}',
                'textColumns' => 'name,sku,slug,description,meta_title,meta_description,meta_keywords',
                'criteria' => [
                    'condition' => 'status = :status',
                    'params' => [
                        ':status' => 1
                    ],
                ],
            ],
        ],
    ],
    'import' => [
        'application.modules.zendsearch.models.*',
    ],
    'component' => [],
    'rules' => [
        '/search' => 'zendsearch/search/search',
    ],
];

</code></pre>


Это описание моделей, которые нужно включить в индекс:

- Page - название модели со следующими параметрами:
- path - алиас пути до модели (для уточнения где лежит эта модель)
- titleColumn - заголовок, который будет выводиться в списке результатов поиска
- linkColumn - поле таблицы, где хранится ссылка на модель
- linkPattern - паттерн ссылки на страницу модели
- textColumns - здесь через запятую нужно перечислить все поля таблицы, которые нужно загнать в индекс Важно! первым нужно указать поле, которое будет выводиться в качестве сниппета для страниц результатов поиска (сниппет формируется путем обрезки текста до 600 знаков, правильнее вывести это значение в свойства модуля, но руки не дошли до этого)
- criteria - Условия фильтрации моделей для индексации

Далее перейти на страницу модуля **http://site.ru/zendsearch/default/index** и обновить индекс поиска.
Индекс поиска по умолчанию хранится в папке **/protected/runtime/search**

После этого можно добавлять виджет формы поиска на страницы сайта:
<pre><code class="php">
    $this->widget('application.modules.zendsearch.widgets.SearchBlockWidget');
</code></pre>

!!! important "Придем на помощь!"
    Если Вам необходима интеграция полнотекстового поиска и/или фасетов (ElasticSearch или Sphinx) - мы можем [это сделать](http://yupe.ru/ecommerce/advanced) для Вас!
    [Напишите нам!](http://yupe.ru/contacts)
