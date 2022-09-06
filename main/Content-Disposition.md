Tags: #web 
Created: 2022-09-06 21:09
References: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition 

# Content-Disposition
This is a header useh when sending [[HTTP]] requests and responses.

In regular http responses, the `Content-Disposition` indicates if the content should be displayed *inline* or *saved locally*.

```
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"
```

In a [[Multipart]] request, the `Content-Disposition` header is used in each of the parts to give informations about the field it applies to.

```
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"
```

## Examples
This would trigger a **Save As** dialog in the browser (for a regular http response):

```
200 OK
Content-Type: text/html; charset=utf-8
Content-Disposition: attachment; filename="cool.html"
Content-Length: 21

<HTML>Save me!</HTML>
```

If you post a html form using `multipart/form-data` this is how the request may look like:

```
POST /test.html HTTP/1.1
Host: example.org
Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```