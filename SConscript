Import('rtconfig')
from building import *

cwd = GetCurrentDir()
path = [cwd]
src = Split('*.c')
group = DefineGroup('ramdisk', src, depend = ['PKG_USING_RAMDISK'], CPPPATH = path)

Return('group')
