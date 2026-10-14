# CDN (Content Distribution / Delivery Network)

- CDN are kind of cache which comes into play for serving LARGE Amount of STATIC Media.
- CDN are usually Distributed Across different geolocations.

## Example

```text
Netflix -------> Movie ------> Nearest CDN Server (Location) ------> Fetch Response from there
```

- India ⟶ CDN: Mumbai
- Europe ⟶ CDN: Amsterdam
- North America ⟶ CDN: California

Netflix: Own CDN (2015-2018: OpenStack)

## Example: WhatsApp Application

Sports Event / Political Event:

- 1 Image / Poster / Video (Content) ⟶ VIRAL
- ("Forwarded Many Times")

```text
Client -------> Server -------> DB

    i1           i2
A ---------> B --------> C
```

Media: Uploaded to S3 with a Hash ⟶ CDN

Before Uploading the same media on S3:

- Check the Hash
- If Already present, Reuse from CDN
- Else, Upload in S3

---

## When NOT to use CDN?

- If System is NOT Large Enough (Not Distributed Across Geolocations)
- Keep static data in a separate domain (OneDrive, GoogleDrive) and using a HTTP Server like NGINX, read from there
- YAGNI: You are Not Gonna Need It: Over Engineering
- KISS: Keep It Simple and Short


