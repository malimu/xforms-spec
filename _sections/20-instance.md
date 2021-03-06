---
title: Instance
---

A `<model>` can have multiple instances as childnodes. The first and required `<instance>` is called the _primary instance_ and represents the data structure of the record that will be created and submitted with the form. Additional instances are called _secondary instances_.

### Primary Instance 

The primary instance should contain a single childnode. In the example below `<household>` will be populated with data and submitted. The primary instance's single child is the **document root** that XPath expressions are evaluated on (e.g. in the instance below the value of `/household/person/age` is 10).

{% highlight xml %}
<instance>
    <household id="mysurvey" orx:version="2014083101">
        <person>
            <firstname/>
            <lastname/>
            <age>10</age>
        </person>
        <meta>
          <instanceID/>
        </meta>
    </household>
</instance>
{% endhighlight %}

Any value inside a primary instance is considered a default value for that question. If that node has a corresponding input element that value will be displayed to the user when the question is rendered.

Nodes inside a primary instance can contain attributes. The client application normally retains the attribute when a record is submitted. There are 3 pre-defined instance attributes:

| attribute     | description
|---------------|------------
| `id`          | on the childnode of the primary instance: This is the unique ID at which the form is identified by the server that publishes the Form and receives data submissions. For more information see [this Form List Specification](https://bitbucket.org/javarosa/javarosa/wiki/FormListAPI). \[required\]
| `orx:version` | on the childnode of the primary instance in the _http://openrosa.org/xforms/_ namespace: Form version which can contain any string value. Like [meta nodes](#metadata) this information is used as a _processing cue_ for the server receiving the submission.
| `jr:template` | on any repeat group node in the _http://openrosa.org/javarosa namespace_: This serves to define a default template for repeats and is useful if any of the leaf nodes inside a repeat contains a default value. It is not transmitted in the record and only affects the behavior of the form engine. For more details, see the [repeats](#repeats) section.

The primary instance also includes a special type of nodes for metadata inside the `<meta>` block. See the [Metadata](#metadata) section




### Secondary Instances - Internal

Secondary instances are used to pre-load read-only data inside a form. This data is searchable in XPath. At the moment the key use case is in designing so-called _cascading selections_ where the available options of a multiple-choice question can be filtered based on an earlier answer.

A secondary instance should get a unique `id` attribute on the `<instance>` node. This allows apps to query the data (which is outside the root, ie. the primary instance, and would normally not be reachable). It uses the the `instance('cities')/root/item[country='nl']` syntax to do this.

{% highlight xml %}
<instance>
    <household id="mysurvey" version="2014083101">
        <person>
            <firstname/>
            <lastname/>
            <age>10</age>
        </person>
        <meta>
          <instanceID/>
        </meta>
    </household>
</instance>
<instance id="cities">
    <root>
        <item>
            <itextId>static_instance-cities-0</itextId>
            <country>nl</country>
            <name>ams</name>
        </item>
        <item>
            <itextId>static_instance-cities-1</itextId>
            <country>usa</country>
            <name>den</name>
      </item>
      <item>
            <itextId>static_instance-cities-2</itextId>
            <country>usa</country>
            <name>nyc</name>
      </item>
      <item>
        <itextId>static_instance-cities-5</itextId>
        <country>nl</country>
        <name>dro</name>
      </item>
    </root>
</instance>
<instance id="neighborhoods">
    <root>
        <item>
            <itextId>static_instance-neighborhoods-0</itextId>
            <city>nyc</city>
            <country>usa</country>
            <name>bronx</name>
        </item>
        <item>
            <itextId>static_instance-neighborhoods-3</itextId>
            <city>ams</city>
            <country>nl</country>
            <name>wes</name>
        </item>
        <item>
            <itextId>static_instance-neighborhoods-4</itextId>
            <city>den</city>
            <country>usa</country>
            <name>goldentriangle</name>
        </item>
        <item>
            <itextId>static_instance-neighborhoods-8</itextId>
            <city>dro</city>
            <country>nl</country>
            <name>haven</name>
        </item>
    </root>
</instance>
{% endhighlight %}

### Secondary Instances - External

[Proposal pending](https://github.com/opendatakit/odk-xform-spec/issues/50).
