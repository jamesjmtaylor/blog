---
title: KMM iOS Presentation layer
date: '2022-11-19T07:39:37-08:00'
---
<img style="float: left; margin:0 1em 0 0; width: 50%" src="/img/blog/desert.jpg"/>  This is the last entry in a multi-part series on Kotlin Multiplatform Mobile.

This entry will cover implementation of the SwiftUI user interface for the iOS app.  I'm using the Army's brand new [ODIN API ](https://odin.tradoc.army.mil/WEG) for a reboot of my WEG iOS and Android applications.  The ODIN API provides in-depth information about a wide array of military equipment.  

When I started work on implementing the KMM repository layer it was under the assumption that both Android and iOS had their own native pagination.  For Android this assumption was correct, and I was able to use the [Android Jetpack Paging Library](https://developer.android.com/topic/libraries/architecture/paging/v3-overview).  iOS, unfortunately, doesn't have a native pagination library.  As I started down the path of writing my pagination logic I very quickly got frustrated that I was creating duplicate logic across the two platforms.  So I did a little digging and found a [KMM multiplatform paging library](https://github.com/kuuuurt/multiplatform-paging).  This library was built to support the Jetpack Paging library out of the box, so very little needed to change for my preexisting Android implementation.  For iOS I just needed to wrap the data in a CommonFlow object, as shown below:

```
    val landPager = Pager(
        clientScope = scope,
        config = PagingConfig(pageSize = PAGE_SIZE, enablePlaceholders = false), // Ignored on iOS
        initialKey = 0,
        getItems = { currentKey, _ ->
            val items = getEquipmentSearchResults(EquipmentType.LAND, currentKey) ?: emptyList()
            PagingResult(
                items = items,
                currentKey = currentKey,
                prevKey = { max(currentKey - 1,0) },
                nextKey = { currentKey + 1 }
            )
        }
    )
    val landPagingData: CommonFlow<PagingData<SearchResult>>
        get() = landPager.pagingData
            .cachedIn(scope) // cachedIn from AndroidX Paging. on iOS, this is a no-op
            .asCommonFlow() // So that iOS can watch the Flow
```

Once that was done I could create my ViewModels.  For iOS I had to create 3 separate VMs, one for each tab/equipment type.  The land equipment view model is below:

```
class LandEquipmentViewModel: EquipmentViewModel {
    override func fetchEquipment() {
        sdk.landPagingData.watch { nullablePagingData in
            guard let list = nullablePagingData?.compactMap({ $0 as? SearchResult }) else {
                return
            }
            self.equipment = list
            self.hasNextPage = self.sdk.landPager.hasNextPage
        }
    }
    override func fetchNextData() {
        sdk.landPager.loadNext()
    }
}
```



<img style="float: right; margin:1em 0 0 1em; width: 50%" src="/img/blog/iosweg.png"/> 

Photo by <a href="https://unsplash.com/@yuli_superson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Juli Kosolapova</a> on <a href="https://unsplash.com/s/photos/desert?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
