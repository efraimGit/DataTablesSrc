# DataTables plug-in for jQuery - with few added futures

This git repository is a fork from dataTables with my added features. each feature added in separate branch to enable you take only what you want.


### ajax.jsonSrc option
this option enable to use dataTables serverSide processing with your standerd api response. e.g. 
{
    status: status,
    error: error.
    response: {
    }
} 
by send the dataTables [returned data](https://datatables.net/manual/server-side#Returned-data) in response property.
usage:
```
"ajax":{
    "url": url,
    "jsonSrc": response // this row
}
```
### api draw function with locally parameter
this feature enable draw the table locally, even in serverSide processing mode.
this option usefull when you want to enable your users to sorting table by selected rows column.
It makes no sense to send the rows to the server and sort them out there, instead of sorting directly in the browser

usage:
I'm using it by remove event listeners from this col head and attach another function like:
```
$('#dataTable').on('init.dt', function () {
        var settings = table.table().context[0];
        if (settings.oFeatures.bServerSide) {
            // remove default sort listener and sort local
            $('th.checkbox-container').off('click.DT keypress.DT').on('click', function (ev) {
                $(ev.currentTarget).blur(); // Remove focus outline for mouse users
                if (ev.currentTarget.classList.contains('default-cursor')) return; // when no checkeds
                var dir = ev.currentTarget.classList.contains('sorting_desc') ? 'asc' : ev.currentTarget.classList.contains('sorting_asc') ? '' : 'desc';
                // tree state ordering, 'asc', 'desc' and no order
                if (dir) table.column(ev.currentTarget).order(dir); else table.order(settings.oInit.order || []);
                table.draw(false, true);
            });
        }
});
```
### container option
this option enable the library calculate autoWidth, even if the table in a collapsed container like tab or modal
in cases that the table redraw when the container collapse in, the columns.adjust() function can take several hundred milliseconds
To make it redundant, you can add the id of container parent, and the calculation will based of his width instead of table width.
to be honest, this option didn't tested enough, be careful with it.

usage:
"container": '#containerID',

### clesses
I'm added a class 'processing_shown' to table when processing div is shown, to enable use it as css selector.
in additional i'm added class 'sorting_disabled' for each column that not sortable.

## Building

DataTables can be built using the [`make.sh`](build/make.sh) script in the [`/build`](build) directory of this repo. Simply check out the repo, cd into the `build` folder and run `bash make.sh --help` to get a full list of the options available for the build process. `bash make.sh build` will be the most common (with `bash make.sh build debug` available for quick testing - it skips the minification steps for speed).

A number of programs are required out your computer to be able to build DataTables:

* Bash
* PHP 5.4+
* [Sass](http://sass-lang.com/install) - CSS compiler
* [Closure compiler](https://github.com/google/closure-compiler) - Javascript compressor
* [JSHint 2.1+](http://jshint.com/install/) - Linter (optional)

The build script assumes that a Mac or Linux environment is being used - Windows builds are not currently directly supported (although would be possible using [Cygwin](https://www.cygwin.com/)). Additionally, you may need to alter the paths for the above programs to reflect where they are installed on your own computer - this can be done in the [`build/include.sh`](build/include.sh) script.

The output files are placed into `built/DataTables/` which is a temporary directory. No changes should be made in that directory as they will be **overwritten** when you next build the software.


## Documentation

Full documentation of the DataTables options, API and plug-in interface are available on the [DataTables web-site](//datatables.net). The site also contains information on the wide variety of plug-ins that are available for DataTables, which can be used to enhance and customise your table even further.


## Support

Support for DataTables is available through the [DataTables forums](//datatables.net/forums) and [commercial support options](//datatables.net/support) are available.


## License

DataTables is release under the [MIT license](//datatables.net/license). You are free to use, modify and distribute this software, but all copyright information must remain.
