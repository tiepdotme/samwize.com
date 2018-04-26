To verify for https://samwize.com/2017/10/05/adding-playground-to-an-existing-project/

_UPDATE APRIL 2018:_

[Cocoapods v1.5.0](http://blog.cocoapods.org/CocoaPods-1.5.0/) started supporting static libraries, and somehow breaking the way frameworks are imported into Playground.

If you encounter `Playground execution failed: error: Couldn't lookup symbols: ...`, then it is likely the modules are not functioning.

The only fix I know now is to revert back to v1.4.0.

```
sudo gem install cocoapods -v 1.4.0
pod deintegrate
pod _1.4.0_ install
```

---
