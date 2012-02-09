# Reading a File Into Memory

Reading the contents of a file into memory is a very common programming task, and, as with many other things, the Node core API provides methods to make this trivial.  There are a variety of file system methods, all contained in the `fs` module.  The easiest way to read the entire contents of a file is with `fs.readFile`, as follows:

    var fs = require('fs');
    var encoding = 'utf8';
    fs.readFile(filePath, [encoding], [callback]);

`encoding` is an optional parameter that specifies the type of encoding to read the file. Possible encodings are 'ascii', 'utf8', and 'base64'. If no encoding is provided, the default is `utf8`.

`callback` is a function to call when the file has been read and the contents are ready - it is passed two arguments, `error` and `data`.  If there is no error, `error` will be `null` and `data` will contain the file contents; otherwise `err` contains the error message.

    function callback(err, data) {
      if (err) { return console.error(err); }
      console.log('file content is:', data);
    }

So if we wanted to read `/etc/hosts` and print it to stdout (just like UNIX `cat`):

    var fs = require('fs')
    fs.readFile('/etc/hosts', 'utf8', function (err,data) {
      if (err) {
        return console.error(err);
      }
      console.log(data);
    });

The contents of `/etc/hosts` should now be visible to you, provided you have permission to read the file in the first place.

Let's now take a look at an example of what happens when you try to read an invalid file - the easiest example is one that doesn't exist.

    fs = require('fs');
    fs.readFile('/doesnt/exist', 'utf8', function (err,data) {
      if (err) {
        return console.log(err);
      }
      console.log(data);
    });

This is the output:

    { stack: [Getter/Setter],
      arguments: undefined,
      type: undefined,
      message: 'ENOENT, No such file or directory \'/doesnt/exist\'',
      errno: 2,
      code: 'ENOENT',
      path: '/doesnt/exist' }

This is a basic Node.js [Error object](/what-is-the-error-object) - it can often be useful to log `err.stack` directly, since this contains a stack trace to the location in code at which the Error object was created.