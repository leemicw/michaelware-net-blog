---
title: Using Marvin.JsonPatch and Swashbuckle to Generate Swagger Documents
tags:
  - Software
date: 2017-06-17 22:55:19
---

We have been working to implement new API documentation.&nbsp; Since ASP.net help pages are not going to be available in .net core we decided to move to Swagger.&nbsp; In case you aren’t familiar with the pipeline, here are the basics.&nbsp; 

ASP.net help pages were generated from the .net source code.&nbsp; Swagger documentation is generated from a JSON document.&nbsp; Swashbuckle is the glue that generates the Swagger JSON from the .net code.&nbsp; It’s beyond the scope of this document to talk about how awesome this stack is, but you should check it out.&nbsp; 

## JSON Patch Documents

We are using the JSON patch documents to allow partial updates to our system.&nbsp; Since parsing of these documents is not built into ASP.net by default, we are using the Marvin.JsonPatch nuget package.&nbsp; This is working great for us, but the format of the JsonPatch&lt;T&gt; object that is the parameter to the controller method does not match the format of the JSON payload you have to send to the PATCH request. For example. 
<pre class="prettyprint">public HttpResponseMessage PatchLead([FromUri] int leadId, [FromBody] JsonPatchDocument&lt;PatchLeadViewModel&gt; patch)</pre>

The patch parameter has an Operations collection that you can process.&nbsp; Since this collection is on the object, Swashbuckle generates the sample payload to look like this.&nbsp; 
<pre class="prettyprint">{
&nbsp;&nbsp; "Operations": [
&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "value": {},
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "path": "string",
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "op": "string",
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "from": "string"
&nbsp;&nbsp;&nbsp;&nbsp; }
&nbsp;&nbsp; ]
}</pre>

Unfortunately the correct JSON Patch payload looks like this. 
<pre class="prettyprint">[
&nbsp;&nbsp; { "op": "replace", "path": "/baz", "value": "boo" },
&nbsp;&nbsp; { "op": "add", "path": "/hello", "value": ["world"] },
&nbsp;&nbsp; { "op": "remove", "path": "/foo"}
]</pre>

## The Solution

Swashbuckle gives us some places we can tie into the JSON generation pipeline so we can customize the output.&nbsp; One of those places is in the SchemaFilter.&nbsp; SchemaFilters implement the ISchemaFilter interface which has a single method with this signature. 
<pre class="prettyprint">public void Apply(Schema schema, SchemaRegistry schemaRegistry, Type type)</pre>

This fires for each of the controller method parameters during the Swagger JSON generation.&nbsp; You can inspect the type, and if needed override the default output from Swashbuckle.&nbsp; This is a class that looks for all parameters that implement JsonPatchDocument&lt;T&gt; and replaces the generated output with a simple JSON Patch document example.&nbsp; 
<pre class="prettyprint">public class JsonPatchDocumentSchemaFilter : ISchemaFilter
{
&nbsp;&nbsp;&nbsp;&nbsp; public void Apply(Schema schema, SchemaRegistry schemaRegistry, Type type)
&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (type.IsGenericType &amp;&amp; type.GetGenericTypeDefinition() == typeof(JsonPatchDocument&lt;&gt;))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema.example = new[] {new Marvin.JsonPatch.Operations.Operation() {op = "replace", path = "/SamplePath", value = "UpdatedValue"}};
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }
&nbsp;&nbsp;&nbsp;&nbsp; }
}</pre>

The last thing you have to do is register the SchemaFilter with Swashbuckle.&nbsp; In the startup.cs file where you enable and configure Swashbuckle, you need to add the following.
<pre class="prettyprint">config.EnableSwagger(c =&gt;
&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; c.SchemaFilter&lt;JsonPatchDocumentSchemaFilter&gt;();
&nbsp;&nbsp;&nbsp;&nbsp; })
&nbsp;&nbsp;&nbsp;&nbsp; .EnableSwaggerUi(c =&gt;
&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...
&nbsp;&nbsp;&nbsp;&nbsp; });</pre>

## Notes

While writing this we were using Marvin.JsonPatch v 0.9.0 and Swashbuckle.Core v5.5.3