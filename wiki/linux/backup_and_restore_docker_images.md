# Backup and restore docker images


In order to fully backup images in a system (for moving or copying them somewhere else) you need five steps:

1. Save the tags for the images
2. Backup the images
3. Transfer the backups
4. Restore the images
5. Restore the tags

This assumes you want to backup all the images on the system together.

The backup part is via [Franklin Piat](https://stackoverflow.com/a/37650072/9001677)

## Save the tags

Using `docker images` we can save the tags of all our images [in the format we desire](https://docs.docker.com/engine/reference/commandline/images/#format-the-output):

```bash
docker images --format "{{.ID}} {{.Repository}}:{{.Tag}}" > /path/to/tags.txt

```

This creates a file similar to this:

```
abcdef012345 ubuntu:16.04
bcdef0123456 repository/image1:latest
bcdef0123456 repository/image1:other_tag
```

## Backup the images
This will create a single tar file holding all the images together.

```bash
docker save $(docker images -q) -o /path/to/images.tar
```

You can later compress this file to save some space. Here I assume you simply have the .tar file.


## Transfer the backups

Move both the `.tar` with the images and the `.txt` with the tags to the target.

## Restore the images

Restore the images using:

```bash
docker load -i /path/to/images.tar
```

## Restore the tags

We now retag each image from our backup:

```bash
while read line; do docker tag $line; done < /path/to/tags.txt
```

# Extra: moving images locally to a different storage driver

Once upon a time, in a system I had to [change the storage driver due to some issues](linux/fixing_docker_storage_driver_issues_in_ubuntu.md).

If I had just defined a different driver, all the images would be "missing" in the new storage driver (although still existing in the old one), so it required a full backup and restore, with some cleaning in the middle.

**Please note**: this procedure _**will**_ destroy all your docker data (not only images), so make sure you understand what you are doing and have backups of everything.

Keep in mind that I did this with *only* images in the system. No running or stopped containers, volumes or anything else.

To migrate from overlay to overlay2:

1. If needed, fix and define the `overlay` storage driver
2. Make a backup of the images *and* their tags
3. Clean up all the images
4. Use `docker system prune` to make sure no leftovers remain in the system
5. Change the storage driver to `overlay2`
6. Restore all the images *and* their tags
