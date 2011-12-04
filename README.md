GettextUtil
===========

Requirement
-----------
Wrap command line `scripts/compile_twig_template_i18n.php` to PHP Class:

    $cmd = " xgettext -j -o messages.po --from-code=UTF-8 -n --omit-header -L PHP -D $(find $cacheDir -type f -iname '*.php')";

    echo "Combining messages..\n";
    system('find locale -type f -iname "*.po" | while read pofile ; do msgcat messages.po $pofile >| $pofile.new ; mv -v $pofile.new $pofile ; done' );
    echo "\n";

    echo "Generating binary format file..\n";
    msgfmt -v locale/zh_TW/LC_MESSAGES/phifty.po
    system('find locale -type f -iname "*.po" | while read pofile ; do echo "Processing " $pofile ; msgfmt $pofile ; done' );

File Structure
--------------

- src/  - class source code
- tests/  - unit testing code
- doc/    - documentation

API Synopsis
------------
Parser locale strings into mo files

    use GettextUtil\GettextParser;
    $gettext = new GettextParser;
    $gettext->addPath('path/to/dir');
    $gettext->omitHeader();
    $gettext->setOutputFile('message.po');
    $success = $gettext->parse();  # get system return value.

To convert po files to mo files:

    use GettextUtil\GettextFormatter;
    $formatter = new GettextFormatter;
    $formatter->scan('locale/en...');

Merge Po files:

    use GettextUtil\GettextPoFile;
    $pofile = new GettextPoFile('messages.po');
    $target_po = $pofile->mergeTo('path/to/target.po');
    $target_po->formatMo();   // generate path/to/target.mo


