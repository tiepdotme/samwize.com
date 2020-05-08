---
layout: post
title: "How to Generate PDF With Images"
date: 2019-07-02T14:22:51+08:00
categories: [Swift]
---

To create PDF, one of the most popular way is to create the HTML, then convert it to a PDF.

This is in essence using [`UIMarkupTextPrintFormatter`](https://developer.apple.com/documentation/uikit/uimarkuptextprintformatter), one of the [provided formatters](https://developer.apple.com/documentation/uikit/printing) to generate PDF.

## Pitfall: It does NOT work for images

Unfortunately, `UIMarkupTextPrintFormatter` can interpret all of HTML markups, but not the images.

Numerous [approaches](https://stackoverflow.com/q/7058556/242682) yield no results, including:

- `img` tag with local file path
- `img` with base64 encoding

It is in no fault to do with the HTML, since that can be displayed correctly in web views.

## Solution: Use WebKit

It turns out a possible solution is to use `WKWebView` to load the html, then access it's `viewPrintFormatter` ([a `UIView` property](https://developer.apple.com/documentation/uikit/uiview/1621835-viewprintformatter)), then generate the PDF.

An extra step needs to be taken to wait for the web view to finish loading the HTML.

### Step 1: Load the web view

```swift
typealias ExportManagerCompletion = (Result<NSData, Error>) -> Void

class ExportManager: NSObject {
  var webView: WKWebView? = nil
  var completion: ExportManagerCompletion!

  func exportPDF(html: String, completion: @escaping ExportManagerCompletion) throws {
      self.completion = completion

      let webView = WKWebView()
      webView.navigationDelegate = self
      webView.loadHTMLString(html, baseURL: nil)
      self.webView = webView
  }
}
```

We created an `ExportManager` here to make things clearer, largely it needs to:

- hold an instance of the web view, though not displaying it
- provide a completion handler (`ExportManagerCompletion`) to return the PDF data
- be a delegate to the web view

## Step 2. Handle when finish loading

```swift
extension ExportManager: WKNavigationDelegate {
    func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
          let formatter = webView.viewPrintFormatter()
          createPDF(formatter)
        }
    }
}
```

Our class extend `WKNavigationDelegate` to handle when the HTML has finished loading.

We access the web view's `viewPrintFormatter`, which works correctly with images!

## Step 3. Render the PDF

Finally, we can render the PDF with a working formatter.

The manager class will use `UIPrintPageRenderer` to render the PDF and then call the completion handler successfully.

```swift
func createPDF(_ formatter: UIViewPrintFormatter) {
  let render = UIPrintPageRenderer()
  render.addPrintFormatter(formatter, startingAtPageAt: 0)

  // Assign paperRect and printableRect
  // A4, 72 dpi
  let paperRect = CGRect(x: 0, y: 0, width: 595.2, height: 841.8)
  render.setValue(paperRect, forKey: "paperRect")
  let padding: CGFloat = 24
  let printableRect = paperRect.insetBy(dx: padding, dy: padding)
  render.setValue(printableRect, forKey: "printableRect")

  // 4. Create PDF context and draw
  let pdfData = NSMutableData()
  UIGraphicsBeginPDFContextToData(pdfData, .zero, nil)
  for i in 0..<render.numberOfPages {
      UIGraphicsBeginPDFPage();
      render.drawPage(at: i, in: UIGraphicsGetPDFContextBounds())
  }
  UIGraphicsEndPDFContext();

  self.completion?(.success(pdfData))
}
```

## Extra: HTML and Base64 encode the image

As mentioned earlier, you can encode an image with base64, and then include it in the HTML. The `<img>` tag can be generated like this.

```swift
func imageBase64Tag(_ image: UIImage) -> String {
    let jpegData = image.jpeg
    let base64EncodedString = (jpegData as NSData).base64EncodedString()
    let src = "data:image/jpeg;base64,\(base64EncodedString)"
    let tag = "<img src=\"\(src)\"/>"
    return tag
}
```
