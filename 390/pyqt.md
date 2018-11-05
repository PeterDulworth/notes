#### QT Designer

1. Use qt designer to create a layout (.ui) file

2. Compile it to a python file with:

   ```bash
   pyuic4 -x UIFILE.ui -o PYTHONFILE.py
   ```

#### Signals

Default signals can be accessed in two ways:

```python
self.connect(self.THING_THAT_EMITS_SIG, QtCore.SIGNAL("NAME_OF_SIGNAL"), handler) # first way
self.THING_THAT_EMITS_SIG.NAME_OF_SIG.connect(handler) # second way
```

For example, the following

```python
self.reticulationScrollArea.verticalScrollBar().rangeChanged.connect(lambda: self.reticulationScrollArea.verticalScrollBar().setValue(self.reticulationScrollArea.verticalScrollBar().maximum()))
```

is equivalent to

```python
self.connect(self.reticulationScrollArea.verticalScrollBar(), QtCore.SIGNAL("rangeChanged(int,int)"), lambda: self.reticulationScrollArea.verticalScrollBar().setValue(self.reticulationScrollArea.verticalScrollBar().maximum()))
```



**Custom Signals**

To emit:

```python
THING_THAT_EMITS_SIG.emit(QtCore.SIGNAL('SIG_NAME'), arg1, arg2)
```

To receive:

```python
self.connect(self.THING_THAT_EMITS_SIG, QtCore.SIGNAL('SIG_NAME'), handler)
```

Note: by default handler is passed all extra arguments. In this case arg1 and arg2. When the signal is emitted 

```python
handler(arg1, arg2)
```

is called.