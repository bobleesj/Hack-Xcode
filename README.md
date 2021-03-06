# Hack-Xcode

## Useful Features iOS Developers Should Know

### Customizing UINavigation & Status Bar
Put these lines of code to AppDeledate.swift under func didFinishLaunchingWithOptions 
```Swift
        UINavigationBar.appearance().barTintColor = UIColor.redColor
        UINavigationBar.appearance().barStyle = UIBarStyle.Default
        UINavigationBar.appearance().tintColor =  UIColor.redColor
        UINavigationBar.appearance().titleTextAttributes = [NSForegroundColorAttributeName: UIColor.redColor, NSFontAttributeName: UIFont(name: "OpenSans-Bold", size: 25)!]
        UIApplication.sharedApplication().statusBarStyle = UIStatusBarStyle.LightContent
        return true
```
### Making a segue by pressing a button 
```Swift
        @IBAction func buttonPressed(sender: AnyObject) {
        performSegueWithIdentifier("goToSecond", sender: self)
        }
        //You must drag the button to another view controller and name the segue as "goToSecond" on the storyboard
```

### Passing Data to another viewController 
In the secondViewController, create a string variable called, "receviedString"
In the firstViewController, create a textfield and button that leads to the secondViewController

```Swift
        override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
                var secondVC: secondViewController = segue.destinationViewController as! secondViewController 
                secondVC.receivedString = self.textField.text!
                }
        }
```
### Location Manager, MapKit, Annotation 
Getting the location of TajMahal. You need to import MapKit 

```Swift
    func getMapTajMahal() {
        //Coordinates
        let tajLat: CLLocationDegrees = 27.175015
        let tajLong: CLLocationDegrees = 78.042139
        let tajCoordinate = CLLocationCoordinate2D(latitude: tajLat, longitude: tajLat)
        
        //Span
        let latDelta: CLLocationDegrees = 0.01
        let longDelta: CLLocationDegrees = 0.01
        let tajSpan = MKCoordinateSpan(latitudeDelta: latDelta, longitudeDelta: longDelta)
        let tajRegion = MKCoordinateRegion(center: tajCoordinate, span: tajSpan)
        
        mapKitView.setRegion(tajRegion, animated: true)
        
        let tajAnnotation = MKPointAnnotation()
        tajAnnotation.title = "Hahah"
        tajAnnotation.subtitle = "Good"
        tajAnnotation.coordinate = tajCoordinate
        
        mapKitView.addAnnotation(tajAnnotation)
    }
```

### Location, Getting User's Current Location

```Swift 
import UIKit
import CoreLocation
import MapKit

class ViewController: UIViewController, MKMapViewDelegate, CLLocationManagerDelegate{

    var locationManager: CLLocationManager = CLLocationManager()
    @IBOutlet weak var mapKietView: MKMapView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.locationManager.delegate = self
        self.locationManager.desiredAccuracy = kCLLocationAccuracyBest
        self.mapKietView.showsUserLocation = true
        self.locationManager.requestAlwaysAuthorization()
        self.locationManager.startUpdatingLocation()
        mapKietView.showsUserLocation = true
        mapKietView.userTrackingMode = MKUserTrackingMode.Follow
    }
   //Location Delegate Methods
    func locationManager(manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let location = locations.last
        let center = CLLocationCoordinate2D(latitude: (location?.coordinate.latitude)!, longitude: (location?.coordinate.longitude)!)
        let region = MKCoordinateRegion(center: center, span: MKCoordinateSpan(latitudeDelta: 0.01, longitudeDelta: 0.01))
        self.mapKietView.setRegion(region, animated: true)
        self.locationManager.stopUpdatingLocation()
    }
    
    func locationManager(manager: CLLocationManager, didFailWithError error: NSError) {
        print("Error" + error.localizedDescription)
    }
}
```
### Adding a pop-over to a view controller
This is useful if you want to add and an invite feature or to indicate a level up. 
1. Make two UIViewControllers. The first controller has a button that will cause the second view to pop up. 
2. On the mainstoryboard, let the secondViewControllerID as "sbPopUpID" (any name you want) 

firstViewController (name: ViewController) 
```Swift 
        import UIKit
        class ViewController: UIViewController {
                @IBAction func showPopUp(sender: AnyObject) {
                        let popOverVC = UIStoryboard(name: "Main", bundle: nil).instantiateViewControllerWithIdentifier("sbPopUpID")
                        as! PopUpViewController
                
                //Adding secondView to the current view 
                self.addChildViewController(popOverVC)
                // Make the secondView's location and coordinate system relative to the firstView
                popOverVC.view.frame = self.view.frame
                self.view.addSubview(popOverVC.view)
                popOverVC.didMoveToParentViewController(self)
                }
        override func viewDidLoad() {
                super.viewDidLoad() 
        }
}
```
secondViewController (name: PopUpViewController) 
```Swift
        import Foundation
        import UIKit
        
        class PopUpViewController: UIViewController {
        override func viewDidLoad() {
                super.viewDidLoad() 
                self.view.backgroundColor = UIColor.blackColor().colorWithAlphaComponent(0.8)
                self.showAnimate()
                }
        @IBAction func closePopUp(sender: AnyObject) {
                self.removeAnimate() 
                }
        
        // Animation of the pop up
        func show Animate() {
                self.view.transform = CGAffineTransformMakeScale(1.2, 1.2)
                self.view.transform = CGAffineTransformMakeRotation(0.23)
                self.view.alpha = 0.0
                UIView.animateWithDuration(0.25, animations: {
                        self.view.alpha = 1.0
                        self.view.transform = CGAffineTransformMakeScale(1.0, 1.0)
                        })
        func removeAnimate() {
                UIView.animateWithDuration(0.25, animations: {
                        self.view.transform = CGAffineTransformMakeScale(1.3, 1.3)
                        self.view.alpha = 0.0 }, completion: {(finished: Bool) in 
                                if (finished) {
                                self.view.removeFromSuperView()
                                }
                        })
                }
        }
```
### Generation Random Number
```Swift 
        func randomInt(min: Int, max: Int) -> {
                return min + Int(arc4random_uniform(UInt32(max-min +1)
```
### Converting NSDate <-> Date(String)
```Swift 
        // NSDate to String 
        let date = NSDate() // Aug 10, 2016, 10:31PM
        let dateformatter = NSDateFormatter()
        dateFormatter.dateFormat = "dd-MM-yyyy"
        let todayDate: String = dateFormatter.stringFromDate(date)
        print(todayDate) // 10-08-2016
        
        // String to NSDate
        let todayDate = "10-08-2016"
        let newDateFormatter = NSDateFormatter()
        var dateFromString = dateFormatter.dateFromString(todayDate)
        print(dateFromString)
```

###Alert Controller 
```Swift
        LogOutAlert = UIAlertController(title: "Want to log out", message "You sure?", preferredStyle: .Alert)
        // Creating LogOut Button
        let logOutAlertAction = UIAlertAction(title: "Log Out", style. Default, handler: { action in 
                let loginVieController: UIViewController = self.storyboard!.instantiateViewControllerWithIdentifir("login")
                self.presentViewController(loginViewController, animated: false, completion: nil) })
        
        // Create Cancel Button 
        let cancelAction = UIAlertAction(title: "Cancel", style .Cancel, handler: { action in 
                print("Cancel")
                self.navigationController?.popViewControllerAnimated(true) })
        
        logOUtAlert?.addAction(logOutAlertAction)
        logOUtAlert?.addAction(cancelAction)
        
        override func viewDidAppear(animated: Bool) {
                super.viewDidAppear(true)
                self.presentViewController(LogOUtAlert!, animated: true, completion: nil) 
                }
```

### Modal View Controllers 
##### Preparing for a Modal Segue 
```Swift
        func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObjects!) {
                if segue.identifier == "GoTOModalVC" {
                        let vc = segue.destinationViewController as MyModalVC {
                }
        }

        // Dismiss
        func dismissViewControllerAnimated(Bool, completion: () -> Void) 
```

##### How is the modal view controller animated?
```Swift 
        var modalTransitionStyle: UIModalTransitionStyle
        .CovereVertical // slies from the bottom (default) 
        .FlipHorizontal // flips 
        .CrossDissolve // presents VC fades 
        .PartialCurl .. like eBook
```

##### Unwind Segue
Going back to the existing viewController (use presented) 
Instead of ctrl-dragging to another MVC. you ctrl-drag the "Exit" button in the same MVC

##### Popover 
Popovers pop an entire MVC over the rest of the screen


### Core Location
Basic object is CLLocation 
##### Properties
        1. Coordinates
        2. Altitude
        3. Horizontal/Vertical Accuracy
        4. var timestamp: NSDate 
        5. var speed: CLLocationSpeed //meter/second, instataneous
        6. var course: CLLocationDirection //degrees, 0 is north, clockwise 
        
##### Where is this location?
```Swift
        var coordinates: CLLocationCoordinate2D
        struct CLLOcationCoordinates2D {
                CLLocationDegrees latitude // a Double
                CLLocationDegrees longitude // a Double
        }
        var altitude: CLLocationDistance // meters
```
##### How close to that latitude/longtitude is this actual location?
```Swift
        var horizontalAccuracy: CLLocationAccuracy // in meters
        var verticalAccuracy: CLLocationAccuracy // in meters
        // There are three ways to get location 
        GPS // A lot of battery
        Wifi node// <database lookup
        Cellular tower  // not very accurate
```
##### Asking CLLocationManager what your hardware can do 
```Swift
        class func authorizationStatus() -> CLAuthorizationStatus // Authortized, denied, or restricted
        class func locationServicesEnabled() -> Bool // user enabled (or not) locatons for your app
        class func significantLocationChangeMonitoringAvailable() -> Bool
        class func isRangeAvailable() -> Bool // device can tell how far it is from beacons 
```
##### Asking the users if you can monitor their location
```Swift
        func requestWhenInUseAuthorization() // you only wnat location data when you're active
        func requestAlwaysAuthorization() // you want location data when you're not active too
        // You must addd an Info.plist entry 
        // NSLocationWhenInUseUsageDescription
        // NSLocationAlwaysUsageDescription
```
##### Accuracy-based continuous location monitoring
```Swift
        var desiredAccuracy: CLLocationAccuracy // always set this as low as will work for you
        var distanceFilter: CLLocationDistance 
        
        func startUpdatingLocation() // drains battery and will hear complaints
        func stopUpdatingLocation() 
        // Get nofitied via the CLLocationManager's delegate
        func locationManager(CLLocationManager, didUpdateLocations: [CLLocation])
```

##### Error Reporting to the delegates 
```Swift
        func locationManager(CLLocationManager, didFailWithError: NSError)
        // Implement
        kCLErrorLocationUnknown // likely temporary, keep waiting (for awhile at least)
        kCLErrorDenide // user refused to allow your application to receive updates
        kCLErrorHeadingFailture // too much local magnetic interference, keep waiting 
```

##### Get updates in the background 
1. "Significant" location change 
```Swift
      func startMonitoringSignificantLocationChanges()
      func stopMonitoringSignificantLocationChanges()
```

2. "Region-based" location monitoring in CLLocationManager
```Swift
        func startMonitoringForRegion(CLRegion)
        func stopMonitoringForRegion(CLRegion)
        let cr = CLCircularRegion(
                center: CLlocationCoordinate2D, 
                radius: CLLocationDistance,
                identifier: String) 
        CLBeaconRegion // detecting when you are near another device 
        //Now get notified via the CLLocationManager's delegate
        func locationManager(CLLocationManager, didEnterRegion: CLRegion)
        func locationManager(CLLocationManager, didExitRegion: CLRegion)
        func locationManager(CLLocationMnager, monitoringDidFailForRegion: CLRegion, withError: NSError)
        //Region-monitoring also works if your appliation is not running
        var monitoredRegions: Set<CLRegion>
        //CLRegions are tracked by name
        //Circular region monitoring size limit 
        var maximumRegionMonitoringDistance: CLLocationDistance { get } 
```

### Map Kit
##### MKMapView 
1. Coorindate
2. Title
3. Subtitle
4. UIButton
5. UIImageView
```Swift
        //Displays an array of objects which implement MKAnnotation
        var annotations: [MKAnnotation[ { get }
        // MKAnnotation protocol
        protocol MKAnnotation: NSObject 
                var cooridnate: CLLocationCoordinate2D { get }
                var title: String? { get } 
                var subtitle: String? { get } 
```
##### What happens when you touch on an annotation (e.g the pin)?
```Swift
        func canShowCallout() -> Bool // if true, it shows title/subtitle 
        func mapView(MKMapView, didSelectAnnotationView: MKAnnotationView) 
        // great to set up MKAnnotationView's callout lazily until it is selected and called
        
        func mapView(sender: MKMapView, viewForAnnoation: MKAnnotation) -> MKAnnotationView {
                var view: MKAnnotationView! = sender.dequeueReusableAnnotationViewWithIdentifier(IDENT)
        if view == nil {
                view = MKPinAnnotationView(annotation: annotation, reuseIdentifier: IDENT)
                view.canShowCallout = true // false
        }
        view.annotation = annotation // yes this happens twice if no dequeue
        return view 
}
```

##### MKAnnotationView Properties
```Swift
        var annotation: MKAnnotation // the annotation
        var image: UIImage // instead of the pin, 
        var leftCalloutAccessoryView: UIView // maybe a UIImageView
        var rightCalloutAccessoryView: UIView // MAYBE A "disclosure" UIButton
        var enabled: Bool // false means it ignores touch events, no delegate method, no callout
        var centerOffset: CGPoint // where is the "head of the pin" in relative to the image
        var draggable" Bool // only works if the annotation's coordinate's property is { get set }
        
        // Selected the annotation view 
        func mapView(MKMapView, didSelectAnnotationView aView: MKAnnotationView) {
                if let imageView = aView.leftCalloutAccessaryView as? UIImageView {
                        imageView.image = ...
                }
        }
```

##### MKMapView
```Swift
        // Confiruing the map view's display type
        var mapType: MKMapType // .Standard, .Satellite, .Hybrid
        // Shwoing the user's current location 
        var showsUserLocation: Bool
        var isUserLocationVisible: Bool
        var userLocation: MKUserlocation
        // Restricting the user's interaction with the map 
        var zoomEnabled: Bool
        var scrollEnabled: Bool
        var pitchEnabled: Bool // 3D
        var rotateEnabled: Bool        
```
##### Size of the map
```Swift
        var region: MKCoordinateRegion
        strct MKCoordinateRegion {
                var center: CLLocationCoordinate2D 
                var span: MKCoordinateSpan 
                }
        struct MKCoordinateSpan {
                var latitudeDelta: CLLocationDegrees
                var longtitudeDelta: CLLocationDegrees
                }
        func setRegion(MKCoordinateRegion, animated: Bool) // animate setting the region 
        
        // Can also set the center point only or set to show annotations 
        var centerCoordinate: CLLocationCoordinate2D
        func setCenterCoordinate(CLLocationCoordinate2D, animated: Bool)
        func showAnnotations([MKAnnotation]. animated: Bool) 
        
        // You can move from SF to NY by zooming out and zooming in 
        func mapView(MKMapView, didChangeRegionAnimated: Bool)
        // Other powerful 
        MKRount
        MKPolyline
        MKOverlay 
```
        
### Create Views

```Swift
        var frame = CGRect(x: 0, y: 0, width: 100, height: 100)
        var blueView = UIView(frame: frame)
        blueView.backgroundColor = UIColor.blueColor()
        
        var imageView = UIImageView(frame: frame)
        imageView.image = UIImage(named: "samily_face")
        imageView.userInteractionEnabled = true 
        view.addSubview(blueView)

```
