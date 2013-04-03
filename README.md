# Sublime Text PHP Coverage Package

A plugin for Sublime Text 2, which visualises PHP code coverage data in
the editor.

## Installation

Put the SublimePHPCoverage folder in your
`~/Library/Application Support/Sublime Text 2/Packages` folder. You can
do this with git:

```bash
$ git clone git://github.com/bradfeehan/SublimePHPCoverage.git "~/Library/Application Support/Sublime Text 2/Packages/SublimePHPCoverage"
```

## Configuration

For some this package will work out of the box, but you'll most likely find
something that you want to customize. The `phpcoverage.sublime-settings` file
has been annotated with examples of common configurations.  Any of the default
settings can be overridden via the user configuration file.

Additionally, settings may be set on a per-project basis in the `.sublime-project`
file. This is especially useful for setting the path to the Clover report as it may differ for each project. To override a setting in a project simply add the key to the `.sublime-project`
settings object, prepended with `phpcoverage_`. For instance, to override
`path_report` you would set `phpcoverage_path_report` in your `.sublime-project`
file.

## Generating Code Coverage Reports

Ensure that your unit testing framework has been configured to output
Clover-formatted code coverage data to the directory specified in your configuation
file. The default path is `build/log/clover.xml`, but you may override this value
via the settings file.

**NOTE:** The coverage file path must be relative to your project's root directory.

### PHPUnit

Reports can be generated on the fly using PHPUnit's [command-line arguments][1]:

```bash
~/myProject$ phpunit --coverage-clover build/log/clover.xml
```
Most, however, will prefer to configure code coverage in PHPUnit's
[XML configuration file][2]:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit>
    <!-- ... -->
    <logging>
        <log type="coverage-clover" target="build/log/clover.xml" />
    </logging>
    <!-- ... -->
</phpunit>
```
### Codeception

Reports can be generated on the fly using Codeception's CLI:

```bash
~/myProject$ codecept run --coverage --xml
```
Assuming you want to update coverage on every Codeception run, you'll probably just
want to modify your `codeception.yml` file to include the following:

```yaml
coverage:
    enabled: true
```

**NOTE:** Codeception does not allow users to set the destination path of Clover
logs at the time of writing. As such, Codeception users must change the path to the
Clover report in settings to `tests/_log/coverage.xml`.

## Usage

The code coverage data will be loaded from the Clover file and
displayed in the margin, with a summary in the status bar:

![Example](http://i.imgur.com/4ASco.png)

Green lines are covered, red are not covered, and non-annotated lines
aren't executable (not counted in code coverage data).

### Hands-Free Mode

Out of the box, every time a PHP file is saved phpunit will be run and
the file markers will be updated.  This will provide you live code coverage
information as you work. In larger projects this may be undesirable as
the time to generate a new report may be significant.  If this is the case
for your project, simply set `coverage_update_on_save` to `false`.

### Manually Triggering a Refresh

You can hit ⌘⇧C to trigger a refresh of the code coverage data for the
currently focused view (tab).

You may also hit ⌘⇧G to trigger the generation of a new clover report.

These options are also available under "PHP Coverage" in the "Tools" menu.

## Troubleshooting

Check that the code coverage file is present and that it has a `<file>`
element for the file in question. Also look in Sublime's console (accessible
via <code>Ctrl + `</code>).

If `coverage_update_on_save` is set to `true` and new reports are not being
generated your system may require that you specify the full system path to
phpunit or the coverage report generator of your choice in
`coverage_update_command`.

[1]: <http://www.phpunit.de/manual/current/en/textui.html#textui.clioptions> "PHPUnit Command-Line Switches"
[2]: <http://www.phpunit.de/manual/current/en/appendixes.configuration.html> "PHPUnit XML Configuration File"
