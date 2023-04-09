# PermissionMate
PermissionMat is an Android library that simplifies the management of permissions in just two simple steps. With its user-friendly interface and streamlined process, PermissionMat makes it easier than ever to handle permissions in your Android applications. Plus, it's even easier to use than the popular PermissionX library!

In addition to simplifying the management of permissions, PermissionMat also provides the ability to manage permission rationales. This means that if a user denies a permission request, the library will automatically show a rationale explaining why the permission is necessary and ask the user to grant it again. With this feature, you can ensure that your app has all the permissions it needs to function properly, while still providing a seamless user experience

#### Simple Example
Create the instance of PermissionMate in create method
```
val pm = PermissionMate.Builder(this)
            .setPermissions(listOf(Manifest.permission.CAMERA))
            .build()
```
Next call start() on any click listener button
```
pm.start()
```
**Note:Don't call the start() in the same function for example creating and calling it in a single button click. If you do that then this will raise an exception**

### Implementation
```
implementation "io.github.farimarwat:permissionmate:1.1"
```

### Step 1 (Place permissions in manifest file)
```
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        tools:ignore="ScopedStorage" />
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.CALL_PHONE"/>
```

### Step 2 (Create instance of PermissionMate inside create)
```
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)
        mContext = this
        val pm = PermissionMate.Builder(this)
            .setPermissions(
                mutableListOf(
                    PMate(
                        Manifest.permission.CALL_PHONE,
                        false,"Phone call permission is required to work app correctly"),
                    PMate(
                        Manifest.permission.CAMERA,
                        true,"Camera permission is required to work app correctly"),
                    PMate(
                        Manifest.permission.READ_CONTACTS,
                        false,"Read Contacts permission is required to work app correctly")
                ).also {
                    if(Build.VERSION.SDK_INT <= Build.VERSION_CODES.Q){
                        it.add(PMate(
                            Manifest.permission.WRITE_EXTERNAL_STORAGE,
                            true,"Storage permission is required to work app correctly"))
                    }
                }
            )
            .setListener(object : PermissionMateListener {
                override fun onPermission(permission: String, granted: Boolean) {
                    super.onPermission(permission, granted)
                    Log.e(TAG,"Permission: $permission Granted: $granted")
                }

                override fun onError(error: String) {
                    super.onError(error)
                    Log.e(TAG,error)
                }

                override fun onComplete(permissionsgranted: List<PMate>) {
                    super.onComplete(permissionsgranted)
                    Log.e(TAG,"Permission Granted: ${permissionsgranted.size}")
                }
            })
            .build()
       
       //Your more code goes here
       ...
    }
```
Finaly call start():
```
 binding.button.setOnClickListener {

            pm.start()
        }
```

### Documentation

#### PMate Data Class
It takes three arguments
- Permission
- Required (If it is required and set to true then it will do not go further until it get this permission)
- Msg (show a rationale explaining message why the permission is necessary)

#### PermissionMateListener
This is an optional callback listener to get results:
```
PermissionMateListener {
                override fun onPermission(permission: String, granted: Boolean) {
                    super.onPermission(permission, granted)
                    Log.e(TAG,"Permission: $permission Granted: $granted")
                }

                override fun onError(error: String) {
                    super.onError(error)
                    Log.e(TAG,error)
                }

                override fun onComplete(permissionsgranted: List<PMate>) {
                    super.onComplete(permissionsgranted)
                    Log.e(TAG,"Permission Granted: ${permissionsgranted.size}")
                }
            }
```

#### Change Log
**Version 1.1**
Minor bugs fixed

**Version 1.0**
Initial release
