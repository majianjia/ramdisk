# ramdisk
A ramdisk block device based on RT-Thread device driver framework.

It works as a block device which allows to be format to most of the filesystems supported by RT-Thread. 

## Examples

This example does:
- Initialise a ramdisk with size of 2MB, block size of 512. 
- Format it using elmFAT and mount it on root directory.
- Then, create a few folders as mounting point for SD card or storage temporary caching files. 

```C
/* Initialize the filesystem */
int filesystem_init(void)
{
    // ramdisk as root
    if(rt_device_find("ramdisk0"))
    {
        // format the ramdisk
        mkfs("elm", "ramdisk0");

        /* mount ramdisk0 as root directory */
        if (dfs_mount("ramdisk0", "/", "elm", 0, RT_NULL) == 0)
        {
            LOG_I("ramdisk mounted on \"/\".");
			
            /* now you can create dir dynamically. */
            mkdir("/sd", 0x777);
            mkdir("/cache", 0x777);
			mkdir("/download", 0x777);
        }
        else
        {
            LOG_E("root folder creation failed!\n");
        }
        return RT_EOK;
    }
    LOG_E("cannot find ramdisk device");
    return RT_ERROR;
}
INIT_ENV_EXPORT(filesystem_init);

int ramdisk_device_init(void)
{
   ramdisk_init("ramdisk0", NULL, 512, 512*4096);
   return RT_EOK;
}
INIT_DEVICE_EXPORT(ramdisk_device_init);

```

