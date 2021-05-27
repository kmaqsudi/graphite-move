# The following steps are to move graphite (whisper DB) to another location

1. Stop Apache  
`sudo systemctl stop httpd`

2. Stop Carbon.   
`sudo systemctl stop carbon-cache`

3. Move whisper directory to new location.  
`rsync -av /opt/graphite/storage/whisper /new_dir/graphite/storage/`

4. Edit /opt/graphite/conf/carbon.conf.  
`Set LOCAL_DATA_DIR to /new_dir/graphite/storage/whisper`

5. Set correct files owner.  
`sudo chown -R www-data:www-data /new_dir/graphite/storage/whisper/`

6. Check if all files moved successfully.  
`find /opt/graphite/storage/whisper/ -print -name '*.wsp' -type f -exec whisper-info.py {} \; | grep Error`
If no file is corrupted move on, otherwise delete corrupted files 

7. Start Carbon.  
`sudo systemctl start carbon-cache`

8. Start httpd.  
`sudo systemctl start httpd`
