## Test Suite Practicess

### Callbacks for each function in Fake/Mock types

When you need to fake any value, behaviour returned from fake, please implement paired callback in fake object as in example:

#### Ex.1 Mocking Void functions

**Declaration**

```swift
struct FakeURLPresenter: URLPresenterProtocol {
	// This part is callback equivalent
    var onPresentURL: ((_ urlPath: URL, _ presentationMode: BMAPushNotificationPresentationMode) -> Void)?
    
    // This if mocked/faked public function
    func presentURL(_ urlPath: URL, in presentationMode: BMAPushNotificationPresentationMode) {
        onPresentURL?(urlPath, presentationMode)
    }
}
```

**Test Suite**

```swift

func testThat_GivenCorrectURL_WhenHandleURL_ThenWillPresentReturnedURLWithPresentationMode() {
	// Other definitions 
	// ...
	
	// Our fake_presentURL callback definition
    self.fakeUrlPresenter.onPresentURL = { (urlPath, presentationMode) in
    	 // Then - You can have many aproach when to assert, but it's not a part of this gist
        XCTAssertEqual("tt://Fixture", urlPath.absoluteString)
        XCTAssertEqual(BMAPushNotificationPresentationMode.modalScreen, presentationMode)
    }
	
	// When 
    _ = self.sut.handleURL(url)
}
```

#### Ex.2 Mocking functions that returns values

**Declaration**

```swift
class FakeClientSourceToURLMapper: ClientSourceToURLMapperProtocol {
	// This part is callback equivalent
    var onMapToURLPath: ((_ clientSource: BMClientSource, _ additionalParams: [AdditionalURLMapperParams: String]) -> String?)?

    // This if mocked/faked public function that return parameter
    func mapToURLPath(from clientSource: BMClientSource, additionalParams: [AdditionalURLMapperParams: String]) -> String? {
        return self.onMapToURLPath?(clientSource, additionalParams)
    }
	
	// ... other functions
}
```

**Test Suite**

```swift
func testThat_GivenCorrectURL_WhenHandleURL_ThenWillMapCorrectClientSource() {
	// ..
    self.fakeClientToURLMapper.onMapToURLPath = { (clientSource, _) in
        return "tt://Fixture"
    }

	// ...
}
```
