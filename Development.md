# Python development

## Get version
```
import pyluksde

pyluksde.get_version()
```

## Open volume
```
import pyluksde

luksde_volume = pyluksde.volume()

luksde_volume.open("image.raw")

luksde_volume.close()
```

**Note that the explicit call to close is not required.**

## Open volume using a file-like object
```
import pyluksde

file_object = open("image.raw", "rb")

luksde_volume = pyluksde.volume()

luksde_volume.open_file_object(file_object)

luksde_volume.close()
```

**Note that the explicit call to close is not required.**

## Also see
```
import pyluksde

help(pyluksde)
help(pyluksde.volume)
```

