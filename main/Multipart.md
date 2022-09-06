Tags: #web 
Created: 2022-09-06 20:09
References: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST https://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2

# Multipart
A multipart [[HTTP]] request is a request that is made up of multiple parts (obviously). These parts are split in multiple parts by some predefined boundary. This type of request is optimal for sending binary data.

One of those requests would look like this:

```
POST /test HTTP/1.1
Host: foo.example
Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```

Note the boundaries and how each part of the request is split by a boundary, which is specified in the `Content-Type` header, prefixed with `--`. The multipart requests ends once the boundary is received with `--` as both prefix and sufix.

A more complex example of a multipart request where the user may send multiple different types of data is illustrated below, starting from this [[HTML]]

```html
<FORM action="http://server.com/cgi/handle"
      enctype="multipart/form-data"
      method="post">
   What is your name? <INPUT type="text" name="submit-name"><BR>
   What files are you sending? <INPUT type="file" name="files"><BR>
   <INPUT type="submit" value="Send"> <INPUT type="reset">
</FORM>
```

If the user selected an image in the second input, here is how the sent data would look like

```
Content-Type: multipart/form-data; boundary=AaB03x

--AaB03x
Content-Disposition: form-data; name="submit-name"

Larry
--AaB03x
Content-Disposition: form-data; name="files"
Content-Type: multipart/mixed; boundary=BbC04y

--BbC04y
Content-Disposition: file; filename="file1.txt"
Content-Type: text/plain

... contents of file1.txt ...
--BbC04y
Content-Disposition: file; filename="file2.gif"
Content-Type: image/gif
Content-Transfer-Encoding: binary

...contents of file2.gif...
--BbC04y--
--AaB03x--
```