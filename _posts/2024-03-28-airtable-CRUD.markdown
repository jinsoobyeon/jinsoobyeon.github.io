---
layout: post
title: "Airtable CRUD"
date: 2024-03-28 00:00:00 +0900
categories: jekyll update
---

There is a very good way to develop a `wireframe` for the CEO of a developer-turned-startup to use for `internal communication`.

Actively use the `airtable`, which is the middle between the database and Excel.

From now on, we will do `CRUD` like a database on the air table.

First, install the airtable library:
{% highlight terminal %}
npm install airtable
{% endhighlight %}

Use the installed library module to connect the airtable:
{% highlight javascript %}
import Airtable from "airtable";

const airtable = new Airtable({
  apiKey:
    "API key generated by airtable",
});
const base = airtable.base("First path right next to the domain during the created base connection");
const table = base("Created table name");
{% endhighlight %}
You have completed all database connection procedures.

If you use the airtable as a `database`, you can `connect directly from the client end`. Save time to create a connection pool and APIs with MVC patterns on the server.

Let's take a look at how to CRUD one by one:

- `Read all records` at the table
{% highlight javascript %}
import { useState } from 'react';

const [records, setRecords] = useState([]);

const listRecords = async () => {
  setRecords(
      Array.from(await table.select().all()).sort(
        (a, b) => new Date(a.fields.CreatedAt) - new Date(b.fields.CreatedAt)
      )
    );
  };
{% endhighlight %}
With this simple code, all records in the table are gotten and stored in the state. The values for each record refer to the field name in the fields properties.

However, there is a caveat. Because the `response comes as a array-like object`, it is converted to an array as above and sorted using the array method.

- `Read a Record` at the table
{% highlight javascript %}
const [record, setRecord] = useState({});

const getRecord = async () => {
  setRecord(await table.find(id));
};
{% endhighlight %}
That's it. Use the id of the record to get each record.

But we don't know the id right away. However, `each record has an id property when getting all records`. This is the same business logic as when developing using RDB.

- `Create a record` in the table
{% highlight javascript %}
const [title, setTitle] = useState("");
const [description, setDiscription] = useState("");
const [body, setBody] = useState("");

const createRecord = async () => {
  const record = await table.create({
    Slug: title.replaceAll(" ", "-"),
    Title: title,
    Description: description,
    Body: body,
  });
}
{% endhighlight %}
Simply send the object with the field name key and the value you want to store. The response is a saved record, as you can see from the code above.

Even the creation time and update `time are automatically generated` by the airtable.

- `Update a record` in the table
{% highlight javascript %}
const updateRecord = async () => {
  await table.update(id, {
    Slug: title.replaceAll(" ", "-"),
    Title: title,
    Description: description,
    Body: body,
  });
}
{% endhighlight %}
The only difference with record creating is that you need to send an id of the record to be updated. Of course, id exists as an attribute in each record when all records are getting.

{% highlight javascript %}
const updateRecord = async () => {
  await table.update(id, {
    Slug: title.replaceAll(" ", "-"),
    Title: title,
  });
}
{% endhighlight %}
Also, `not all fields need to be updated`, please refer to the code above.

- `Delete a record` in the table
{% highlight javascript %}
const deleteRecord = async () => {
  await table.destroy(id)
}
{% endhighlight %}
Delete the record using the id of the record. We already know the id when we get all the records.

`I sincerely hope that your business will be successful.`