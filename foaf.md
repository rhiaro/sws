# Exercise in publishing and linking data

Getting your data on the Semantic Web is as simple as putting a text file containing some triples on a server.

## FOAF

[FOAF](http://xmlns.com/foaf/0.1/) is the friend-of-a-friend ontology. It's a small set of terms used for describing people... and the connections between them. For example:

```
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix pos: <http://www.w3.org/2003/01/geo/wgs84_pos#> .

<http://rhiaro.co.uk/about#me>
  a foaf:Person ;
  foaf:name "Amy Guy" ;
  owl:sameAs <http://www.bbc.co.uk/things/e6a0941e-f964-4a78-a2ea-3d47c6405df6> ;
  foaf:img <https://en.gravatar.com/userimage/5236560/4c00a524a0fd157160742d80907349ae.png> ;
  foaf:homepage <http://rhiaro.co.uk>;
  foaf:workplaceHomepage <http://www.inf.ed.ac.uk/people/students/Amy_Guy.html>; 

  foaf:account [
      a foaf:OnlineAccount ;
      foaf:accountServiceHomepage <http://twitter.com/> ;
      foaf:accountName "rhiaro"
  ];

  foaf:basedNear <http://dbpedia.org/resource/Edinburgh>, [
      a pos:Point;
      pos:lat "55.9531"^^xsd:float;
      pos:long "-3.1889"^^xsd:float
  
  ];

  foaf:knows   
    <http://www.bbc.co.uk/things/a2d48df4-79e9-4da6-8e0b-5a374c84ec72>, 
    <http://dbpedia.org/resource/Tomska> 
  .
```

If the external URIs in my FOAF profile are valid and set up correctly, an agent can follow links from my profile to other peoples', and further (the "follow your nose" technique of traversing linked data).

## URIs

I made a URI for myself: `http://rhiaro.co.uk/about#me`. The domain is under my control, so I can make sure that it returns something appropriate to a browser or a script. If you own your own domain name, you can use that. But if not, you've always got `http://homepages.inf.ed.ac.uk/your-student-id/me`.

## Publishing your profile

Once you've picked a URI, write a FOAF profile in your favourite text editor. You can amend the one above. You could add links, using the correct ontology terms and URIs, to topics you're interested in, places you've travelled, books you've read. All of these links can be traversed to build up a fuller picture, without you having to publish all of the data yourself; you only need to create the connections.

Save it as `foaf.ttl`. You can put it on the server space provided to you by Informatics, by dumping it in the `/public/homepages/<user>/web` folder on your DICE machine.

Now when you visit `http://homepages.inf.ed.ac.uk/your-student-id/foaf.ttl` your triples will be returned!

## Bonus: linking with your friends

Add `foaf:knows` links to the URIs of other people you know in the SWS class who have set up their FOAF profiles. Then we can visualise the lot of you. (If you don't know anyone else in the class, you can either talk to someone in the next lecture and get their URI, or link to: `http://homepages.inf.ed.ac.uk/s1158216/me`).

## Bonus: content negotiation

I think you can put a `.htaccess` file in your homepages web root. If so, you can use this to return something different depending on the HTTP request.

* If a human navigates to your URI (`http://homepages.inf.ed.ac.uk/~your-student-id/me`) in a browser, they probably don't want to see RDF. So you can return HTML instead, that will be rendered by the browser. This could be just a nice view of the contents of your FOAF profile, or anything you want.

* If a linked-data-traversing software agent dereferences your URI, they'll probably send the (for example) `Accept: text/turtle` header, indicating they want RDF. So to them, you can return your `foaf.ttl` file.

```
RewriteEngine on
RewriteBase /your-student-id

# Serve foaf.ttl when RDF (turtle) is requested
RewriteCond %{HTTP_ACCEPT} ^.*text/turtle.*
RewriteRule ^me$ foaf.ttl [R=303,L]

# Serve html by default 
RewriteRule ^me$ profile.html [R=303,L]

```

Save this as `.htaccess` in your `/web` folder alongside your `foaf.ttl` file and don't forget to create `profile.html` as well.

Now try visiting `http://homepages.inf.ed.ac.uk/your-student-id/me` in a browser. You should be redirected to `profile.html`.

Now try `curl -L -H "Accept: text/turtle" http://homepages.inf.ed.ac.uk/your-student-id/me` at the command line. Your RDF should be returned. (The `-L` flag tells `curl` to follow redirects and the `-H` flag lets you pass the Accept header).
