# SWS mini task

## Metadata 

*Note: [Slides from the SWS Metadata lecture](http://www.inf.ed.ac.uk/teaching/courses/sws/lectures-15/SWS-04-Metadata.pdf)*

1. Think of your favourite book. If you can't think of one, pick something off the [SWS reading list](http://www.inf.ed.ac.uk/teaching/courses/sws/#course-info).

2. Take a look at the [Dublin Core documentation](http://purl.org/dc/terms/) and the [Guidelines for encoding bibliographic information with Dublin Core](http://dublincore.org/documents/dc-citation-guidelines/).

3. Write down as much metadata about your book as possible using Dublin Core terms, and note the Dublin Core namespaces you use. Use *as many properties as possible*. Eg. (this example uses a only small subset of possible properties):

    **Namespaces:**

    * dc = http://purl.org/dc/terms/creator/
    * dctype = http://purl.org/dc/dcmitype/
    

    | Dublin Core term | Value |
    | ---------------- | ----- |
    | dc:title | "Weaving the Web: The Original Design and Ultimate Destiny of the World Wide Web by its inventor" |
    | dc:creator | "Tim Berners-Lee" |
    | dc:type | dctype:PhysicalObject |
    | dc:publisher | "Harper Publications" |
    | ... | "..." |

4. Look through your metadata and identify entities that ought to have a unique identifier of their own, not forgetting the book itself. Create a unique identifier (ie. URI) for your book; you can make up your own, or see if you can find it somewhere like [DBPedia](http://dbpedia.org). Replace other values that are strings (aka Literals) with URIs as appropriate, eg (the following is in the [Turtle](http://www.w3.org/TR/turtle/) syntax of RDF):
    
    ```
    @prefix dc: <http://purl.org/dc/terms/creator/> .
    @prefix dctype: <http://purl.org/dc/dcmitype/> .

    <http://dbpedia.org/resource/Weaving_the_Web> dc:title "Weaving the Web: The Original Design and Ultimate Destiny of the World Wide Web by its inventor" .
    <http://dbpedia.org/resource/Weaving_the_Web> dc:creator <http://www.w3.org/People/Berners-Lee/card#i> .
    <http://dbpedia.org/resource/Weaving_the_Web> dc:type <dctype:PhysicalObject> .
    <http://dbpedia.org/resource/Weaving_the_Web> dc:publisher <http://dbpedia.org/resource/Harper_(publisher)> .
    ```

5. Save your turtle in a file called `book.ttl` and publish it in your UoE webspace by copying it in the `/public/homepages/<user>/web` folder on your DICE machine. Now when you visit `http://homepages.inf.ed.ac.uk/your-student-id/book.ttl` you'll see your triples (although some browsers will try to force you to download the file, or you might have to try 'view-source' to see the output).

6. Validate your turtle to check it is syntactically correct. There are many validators, you could try [this one](http://rdf-translator.appspot.com/) ('translate' from N3 to N3). Fix any syntax errors that may occur.

*Note about DBPedia URIs: When you visit a DBPedia URI you are redirect to a /page/ address. This is the URI for the webpage, not the thing the page is about. Use /resource/ in place of /page/ for the URI of the thing.*
