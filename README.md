trace functions Android:
frida-trace -U -i  -f com.package.app -l script.js --no-paus

# Print all clases used by application
```
Java.perform(function() {
    Java.enumerateLoadedClasses({
        onMatch: function(className) {
            console.log(className);
        },
        onComplete: function() {}
    });
});

```
# Call methods on existing clases:
``` 
Java.perform(function () {
	Java.choose("com.microsoft.class", { 
		"onMatch":function(instance){
				//inst.method();
				console.log("[*] Instance found, result: ", instance);
			
		},
		"onComplete":function() {
			console.log("[*] Finished heap search")
		}
	});
});
```
# Override method calls:
```
Java.perform(function () {
	const cls = Java.use("com.microsoft.class");
	cls.checkServerTrusted.implementation = function() {
        return;
    }
});

```



# Hook Constructor
```
Java.use('java.lang.StringBuilder').$init.overload('java.lang.String').implementation = function(stringArgument) {
      console.log("c'tor");
      return this.$init(stringArgument);
};
```
# hook to SecretKeySpec
``` 
Java.perform(function () {
    var SecretKeySpec = Java.use('javax.crypto.spec.SecretKeySpec');
    SecretKeySpec.$init.overload('[B', 'java.lang.String').implementation = function(p0, p1) {
        console.log('SecretKeySpec.$init("' + bytes2hex(p0) + '", "' + p1 + '")');
        return this.$init(p0, p1);
    };
});
function bytes2hex(array) {
    var result = '';
    console.log('len = ' + array.length);
    for(var i = 0; i < array.length; ++i)
        result += ('0' + (array[i] & 0xFF).toString(16)).slice(-2);
    return result;
}
		
```
