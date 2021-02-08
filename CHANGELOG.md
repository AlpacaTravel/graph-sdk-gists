# Changelog

## February, 2021

- Added query examples for itinerary directions, and how to contextualise and
  connect locations of an itinerary

### Breaking Changes

We have opted to implement a breaking change on our media mutations which are
currently not in use. This is to stengthen consistency of our API design which
we would like to ensure as early in the process.

- Updated key from "media" to "mediaResource" in FinalizeMediaUploadPayload and
  UpdateMediaResourcePayload