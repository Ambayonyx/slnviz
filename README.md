
# slnviz

![CI](https://github.com/josteink/slnviz/workflows/CI/badge.svg)

slnviz is a command-line utility to help visualize Visual Studio and
msbuild build-dependencies and graphs.


## features

slnviz in a nutshell:

- command-line driven.
- exports a [GraphViz](http://graphviz.org/) DOT-file from a Visual Studio SLN-file.
- highlights projects whose dependencies are not found in the solution.
- ability to filter redundant transistive dependencies.
- ability to exclude certain kinds of projects (test, shared, etc) from
  graph.
- ability to highlight specific projects, and dependency-paths in the graph.

## dependencies

### python

slnviz is written in Python and targets Python 3. No additional modules needs to
be installed.

It seems to work with Python 2.7 as well, but that's not a supported target.

### graphviz

If you want to visualize the graph, you need to have
[GraphViz](http://graphviz.org/) installed, or use a online service
like [viz-js](http://viz-js.com/).

## usage

The following example shows how slnviz is intended to be used:

````sh
git clone https://github.com/josteink/slnviz
cd slnviz
./slnviz.py -i ../your_repo/your_solution.sln -o your_solution.dot
dot -Tsvg -o your_solution.svg your_solution.dot
# open svg-file in your preferred viewer
````

To list all parameters and options use the `-h` flag:

````sh
./slnviz.py -h
````

## themes
The output can be themed with the `--theme`  parameter. The default theme is `dark`.  

Supported themes are:

* dark (default)
* light

These themes are defined in the file `themes.ini` in the script folder. You can define your own themes by creating a themes.ini in the folder where you call the application.

### style attributes

With the parameter ` --style attribute value` the dot rendering can be modified even per style attribute. These attributes override the definitions in the theme, so you can combine the usage of `--theme` and `--style` to tweak existing themes to your preference.

### diagram style attrbutes
The following global attributes are supported

| Attribute name                         | Description                                                  | Default value |
| -------------------------------------- | ------------------------------------------------------------ | ------------- |
| bgcolor                                | Diagram background color.                                    | `#222222`     |
| dependency.color                       | Dependency line color.                                       | `#ffffff`     |

### project sytle attributes

When defining the style attribute use as name <class>.<attrbibute>. E.g. `--style project.highlight.fontcolor #123456`

The values in the table below show the default values.

|                                  | attribute =>                                      | style                  | fillcolor                    | fontcolor                    | linecolor              |
| -------------------------------- | ------------------------------------------------- | ---------------------- | ---------------------------- | ---------------------------- | ---------------------- |
| **class**                        | **class description**                             | Fill style of the node | Background color in the node | Foreground color in the node | Line color of the node |
| **project**                      | default project style                             | `filled`               |                              | `#ffffff`                    | `#ffffff`              |
| **project.highlight**            | selected by argument to be highlighted            | `filled`               | `#30c2c2`                    | `#000000`                    | `#000000`              |
| **project.is_missing project**   | project which can not be found on the disk        | `filled`               | `#f22430`                    | `#000000`                    | `#000000`              |
| **project.has_missing_projects** | project depends on projects missing on the disk   | `filled`               | `#c2c230`                    | `#000000`                    | `#000000`              |
| **project.has_invalid_format**   | project was found but can not be parsed correctly | `filled`               | `#ffff00`                    | `#ff0000`                    | `#ff0000`              |

