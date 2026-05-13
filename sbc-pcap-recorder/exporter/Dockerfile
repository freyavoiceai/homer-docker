FROM alpine:3.19

# tshark + wireshark-common gives us mergecap/editcap/tshark.
# This slicer reads raw SIP captures (not HEP), so Alpine's tshark
# is sufficient — the missing HEP3 dissector doesn't matter here.
# busybox httpd handles HTTP + CGI under /cgi-bin/ with no config.
RUN apk add --no-cache \
        bash \
        coreutils \
        busybox-extras \
        tshark \
        wireshark-common \
        tzdata

RUN mkdir -p /var/www/cgi-bin
COPY slice /var/www/cgi-bin/slice
RUN chmod 0755 /var/www/cgi-bin/slice

EXPOSE 8080

# -f foreground, -v verbose access log, -p port, -h docroot.
# busybox httpd auto-executes anything under <docroot>/cgi-bin/ as CGI.
# Runs as the container's current user (root) so the CGI can read /pcap.
CMD ["httpd", "-f", "-v", "-p", "8080", "-h", "/var/www"]
