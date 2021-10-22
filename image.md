Image Service API
======

## Service Description
Image Service API allows uploading of product images to be stored and used within the Zettle applications.

The service returns a `imageUrl` that can be added to a desired product using the Product Library API.
The images can be uploaded using a byte array or a static URL, the image will be eventually available for lookup from
within the Zettle apps or via the `imageUrls` returned by the service.

For upload restrictions, see [Swagger documentation](https://image.izettle.com/swagger).

> **Note:** JPG does not exist as an imageFormat, you must use JPEG for .jpg images.

## URL
https://image.izettle.com/v2/images

## Swagger Documentation
https://image.izettle.com/swagger

---

## Example Request

```http
POST https://image.izettle.com/v2/images/organizations/{organizationUuid}/products`
```
```json
{
  "imageFormat": "JPEG",
  "imageData": null,
  "imageUrl": "https://example.com/image.jpg"
}
```

## Example Response

```json
{
  "imageLookupKey": "0SFqIEwSEeWQb_kt6ixqNQ-10TukFTMEaekExOP02bdsw.jpeg",
  "imageUrls": [
    "https://image.izettle.com/productimage/o/0SFqIEwSEeWQb_kt6ixqNQ-10TukFTMEaekExOP02bdsw.jpeg",
    "https://image.izettle.com/productimage/L/0SFqIEwSEeWQb_kt6ixqNQ-10TukFTMEaekExOP02bdsw.jpeg"
  ]
}
```
Where `o` is a `2000*2000px` image and `L` is a `560*560px` image.

To apply this uploaded image on a product, use the first URL provided in the `imageUrls` array.
