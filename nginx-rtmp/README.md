This README provides information on setting up and configuring nginx-rtmp for streaming media using the RTMP protocol.

# Configuration:

 The nginx-rtmp binary comes with a default configuration suitable for basic streaming. However, you may want to customize the configuration based on your requirements. Update the nginx.conf file in the conf directory.

 Here are some configurations:

```
rtmp {
	out_queue           4096;
    out_cork            8;
    max_streams         128;
    timeout             15s;
    drop_idle_publisher 15s;
    log_interval 5s;
    log_size     1m;
	
    server {
        listen 1935;

        chunk_size 4000;
        application myapp  {
            live on;
			gop_cache on;
        }
    }
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    server {
        listen       82;
 
		location /stat.xsl {
            root html;
        }
		location /stat {
            rtmp_stat all;
            rtmp_stat_stylesheet stat.xsl;
        }
		location / {
            root html;
        }
		
		location /live {
            flv_live on;
			chunked_transfer_encoding  on; #open 'Transfer-Encoding: chunked' response
			add_header 'Access-Control-Allow-Credentials' 'true'; #add additional HTTP header
			add_header 'Access-Control-Allow-Origin' '*'; #add additional HTTP header
			add_header Access-Control-Allow-Headers X-Requested-With;
			add_header Access-Control-Allow-Methods GET,POST,OPTIONS;
			add_header 'Cache-Control' 'no-cache';
        }
    }
}
```
 
This configuration sets up an RTMP server on port 1935 and an HTTP server on port 80 with a live streaming application (myapp). Adjust the configuration based on your specific needs.

# Usage
 ## Verify Configuration:

 Verify that the RTMP server is running by checking the RTMP status page at `http://your-server-ip/stat`.

 ## Streaming Endpoint:

 Use the RTMP streaming endpoint `rtmp://your-server-ip:1935/myapp` for streaming purposes.

 ## Access Live Stream:

 Access the live stream using the HTTP endpoint `http://your-server-ip/live`.
