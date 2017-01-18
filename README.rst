mongoengine-cleaner
===================

A heritable class desined for work with `Mongoengine <http://mongoengine.org>`_ documents. It streamline the validation and correction of the raw MongoDb after reading but before saving.

Full documentation not yet developed.

Declare a Simple Model
----------------------

.. code-block:: python

    import mongoengine as me
    import mongoengine-cleaner as mec

    class Post(me.Document, mec.Cleaner):
        title = me.StringField(max_length=120, required=True, correction=mec.Str)
        tags = me.ListField(me.StringField(), allow_null=False, correction=mec.Skip)
        author = me.StringField(required=True, correction="unknown")


Or Write Something Fancy
------------------------

.. code-block:: python

    import mongoengine as me
    import mongoengine-cleaner as mec

    def author_check(doc, field_name, good_type_match):
        if not good_type_match or doc.author=="":
            doc.author = "author missing"
        if doc.author is None:
            doc.author = "author unknown"
            doc.tags.append("anonymous")
        return doc

    class Post(me.Document, mec.Cleaner):
        title = me.StringField(max_length=120, required=True, correction=mec.Str)
        tags = me.ListField(me.StringField(), correction=mec.SkipNull)
        author = me.StringField(required=True, correction=author_check)

Invoke the Cleanup Method
---------------------------

.. code-block:: python

    post_list = Post.objects(tags='mongodb').cleaner
    other_list = Post.objects(tags='other').cleaner(limit=['title'])

Get It Now
----------
::

   [TBD]

License
-------

MIT licensed. See the bundled LICENSE file for more details.