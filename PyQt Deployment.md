#Mac Ports and py2app deployment

**Find the absolute path of py2applet**

```bash
sudo find / -name "py2applet" -type f
```

**Mac ports python location:**

```bash
/opt/local/bin/python2.7
```

```bash
port select --list python # list all port pythons
```



Traceback (most recent call last):
  File "/Users/Peter/PycharmProjects/ALPHA/dist/main.app/Contents/Resources/__boot__.py", line 144, in <module>
    _run()
  File "/Users/Peter/PycharmProjects/ALPHA/dist/main.app/Contents/Resources/__boot__.py", line 123, in _run
    exec(compile(source, path, 'exec'), globals(), globals())
  File "/Users/Peter/PycharmProjects/ALPHA/dist/main.app/Contents/Resources/main.py", line 8, in <module>
    from raxmlOutputWindows import allTreesWindow, donutPlotWindow, scatterPlotWindow, pgtstWindow, robinsonFouldsWindow, heatMapWindow, bootstrapContractionWindow, dStatisticWindow, msRobinsonFouldsWindow, msPercentMatchingWindow, msTMRCAWindow, windowsToInfSitesWindow, lStatisticWindow
  File "raxmlOutputWindows/allTreesWindow.pyc", line 1, in <module>
  File "raxmlOutputWindows/standardWindow.pyc", line 2, in <module>
  File "module/plotter.pyc", line 9, in <module>
  File "Bio/Graphics/GenomeDiagram/__init__.pyc", line 17, in <module>
  File "Bio/Graphics/GenomeDiagram/_Diagram.pyc", line 29, in <module>
  File "Bio/Graphics/GenomeDiagram/_LinearDrawer.pyc", line 32, in <module>
  File "reportlab/graphics/shapes.pyc", line 12, in <module>
  File "reportlab/platypus/__init__.pyc", line 7, in <module>
  File "reportlab/platypus/flowables.pyc", line 32, in <module>
  File "reportlab/lib/styles.pyc", line 28, in <module>
  File "reportlab/rl_config.pyc", line 45, in <module>
  File "reportlab/rl_config.pyc", line 17, in _defaults_init
  File "reportlab/lib/utils.pyc", line 243, in rl_exec
  File "<string>", line 1, in <module>
  File "<string>", line 1, in <module>
ImportError: No module named rl_settings



Traceback (most recent call last):
  File "/Users/Peter/PycharmProjects/ALPHA/dist/main.app/Contents/Resources/__boot__.py", line 464, in <module>
    _run()
  File "/Users/Peter/PycharmProjects/ALPHA/dist/main.app/Contents/Resources/__boot__.py", line 458, in _run
    exec(compile(source, script, 'exec'), globals(), globals())
  File "/Users/Peter/PycharmProjects/ALPHA/source/main.py", line 1054, in <module>
    form = PhyloVisApp()  # We set the form to be our PhyloVisApp (design)
  File "/Users/Peter/PycharmProjects/ALPHA/source/main.py", line 36, in __init__
    os.mkdir('plots')
OSError: [Errno 13] Permission denied: 'plots'