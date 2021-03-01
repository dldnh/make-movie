# make-movie
see https://stackoverflow.com/a/36290742

to test it, I added this function to a UICollectionViewController that's set up to select images from a UIImageView
(it's kind of a mess, but I was just trying to test the image-to-video stuff before I port it to AppKit)

    override func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        guard let cell = collectionView.cellForItem(at: indexPath) as? AssetsViewControllerCell else {
            fatalError("Failed to find cell as index path \(indexPath)")
        }
        let assetId = cell.representedAssetIdentifier
        guard let asset = self.asset(identifier: assetId) else {
            fatalError("Failed to find asset with identifier \(assetId)")
        }
        let imgMgr = PHImageManager.default()
        let imageOptions = PHImageRequestOptions()
        imageOptions.resizeMode = .exact
        imageOptions.deliveryMode = .highQualityFormat
        imageOptions.isNetworkAccessAllowed = true
        imageOptions.isSynchronous = false
        imgMgr.requestImage(for: asset, targetSize: PHImageManagerMaximumSize, contentMode: .default, options: imageOptions) {
            (image: UIImage?, dictionary: [AnyHashable: Any]?) in
            guard let image = image else {
                print("Didn't get it")
                return
            }
            makeMovie(size: image.size, images: [image])
        }
    }
