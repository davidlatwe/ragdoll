<p style="text-align: center" ><img class="boxshadow no-max-height" width=400 src=https://user-images.githubusercontent.com/47274066/103476508-7d7e2780-4dae-11eb-86c9-c099a08f1314.png></p>

<br>

## Overview

Let's cover the basics..

- Ragdoll requires a commercial licence for commercial use
- Ragdoll is free for non-commercial use
- Ragdoll is free for testing within a commercial environment

About activation..

- A commercial licence is activated with a `Product Key`
- Get a `Product Key` by [purchasing a licence](https://ragdolldynamics.com/pricing)
- A non-commercial licence is activated automatically on launch
- A non-commercial licence expires 30 days after activation

**See also**

- [Managing Floating Licences](/floating-licence)

<br>

## Ragdoll Apprentice

Once activated without a `Product Key`, Ragdoll enters "Apprentice Mode", and is limited to the following.

- Non-commercial projects
- Ragdoll Apprentice cannot be used in the same pipeline as commercial versions of Ragdoll
- Ragdoll Apprentice uses its own file format for Maya scenes

See the [Ragdoll EULA](https://ragdolldynamics.com/eula) for details.

<br>

## FAQ

Let's dive into specifics.

<br>

#### How does it work?

On first launch, Ragdoll will try and connect to the Ragdoll Licence Server and register your trial version. This version is node-locked to the particular machine you are on.

Once you've acquired a product key, you can either:

1. Click the `Ragdoll` menu item (bottom)
2. Enter your product key
3. Click `Activate`

Or if you prefer:

```py
from ragdoll import licence
licence.activate(key)
```

<br>

#### What happens when my trial expires?

Any `rdScene.enabled` attribute will be set to `False`.

Scenes will still load just fine and nothing else in your scene is affected. Once activated, the `.enabled` attribute will return to normal.

<br>

#### Can I renew my trial licence?

Possibly. Reach out to us if this is relevant to you.

<br>

#### Can I open scenes made with the trial version in the commercial version?

No.

Files made with Ragdoll Apprentice will appear scrambled with a commercial version. Ragdoll Apprentice is however able to read files saved with a commercial version.

<br>

#### Can I use my licence on more than one machine?

- Yes, for Ragdoll Personal
- No, for Ragdoll Complete, Unlimited and Batch

With Ragdoll Personal, you can activate and use each Ragdoll licence on up to 3 machines. You just can't run a simulation on more than 1 machine per 1 licence at any given time, that could lead to suspension of the licence.

<br>

#### Can I move a licence between two machines?

Yes.

You can hit the `Deactivate` button (which is same as the `Activate` button once you've activated) and the activation will be released.

<br>

#### Do I need an internet connection to use Ragdoll?

No.

Activation can happen either offline or online, online happening from within Maya at the click of a button and offline being a 4-step process, [see below](#can-i-activate-offline).

<br>

#### How do I use my floating licence?

Floating licences have two parts.

1. A self-hosted licence server
2. The Ragdoll plug-in

Whenever Ragdoll is loaded from Maya, it connects to your licence server in order to "lease" a licence. If you own 10 licences, then one of them will remain leased until the plug-in is unloaded from Maya.

**See also**

- [Managing Floating Licences](/floating-licence)

<br>

#### Can I activate offline?

Yes.

See [Offline Activation](#offline-activation) below.

<br>

#### When exactly is internet required?

A connection is made in one of two separate occasions.

1. Calling `ragdoll.licence.install()` from Python
2. On simulating any frame

`install()` is typically called when the plug-in is loaded and menu is installed.

That is, Maya can open a scene with Ragdoll in it without making a connection to the internet if neither of these things happen. This means you can simulate on one machine, bake or otherwise disable the solver and send it off to a farm (e.g. local or cloud) without worrying about licences.

The alternative would be having to erase any trace of Ragdoll from a scene which would be such a pain.

<br>

#### Can I manage my licence via Python?

Sure can, see below.

<br>

## Licence API

As a user, you'll generally use the UI. But the UI is ultimately making calls to Python (which is making calls to C++) and you can bypass the UI using these same calls.

```py
from ragdoll import licence

# Called once before calling any other licencing function
# This is automatically called on Ragdoll Python initialisation
# and simulation start, but needs calling manually if simulation
# hasn't yet started.
licence.install()

# Retrieve the currently activated product key
licence.current_key()

# Activate using your product ket
licence.activate(key)

# Activation for those without access to Internet
licence.activation_request_to_file(key, fname)
licence.activate_from_file(fname)

# Deactivate whatever key is currently activated
licence.deactivate()

# Deactivate offline, to e.g. move a licence from one machine to another
licence.deactivation_request_to_file(fname)

# Dictionary of useful information
data = licence.data()

{
	# Same as current_key
    "key": "Your-Key",

    # Is the current licence activated?
    "isActivated": True,

    # Is the current licence a trial licence?
    "isTrial": False,

    # Has the licence not been tampered with?
    "isGenuine": True,

    # Has the licence been verified with the server
    # (requires a connection to the internet)?
    "isVerified": True,

    # How many days until this trial expires?
    "trialDays": 23
}
```

<br>

## Offline Activation

Lifetime licences may be activated offline.

??? info "What about monthly licences?"
    These require an internet connection. Floating licences require internet only for the licence server itself, meaning your workstations can remain disconnected and protected. Node-locked licences require internet on the machine being activated.

1. Launch the Offline Activation Wizard
2. Copy/paste the activation request into https://ragdolldynamics.com/offline
3. Click Activate
4. Copy/paste the activation *response* into the Wizard
5. Profit

https://user-images.githubusercontent.com/2152766/151695125-ded8c7f2-077d-4db6-8ea9-499a30f7c5c3.mp4 controls

<br>

## Offline Deactivation

Similar to Activation, Deactivation happens via the Offline Deactivation Wizard.

1. Launch the Offline Deactivation Wizard
2. Copy/paste the deactivation request into https://ragdolldynamics.com/offline
3. Click Deactivate
4. Profit

!!! warning "Caution"
    Once you press `Deactivate`, then from Maya's perspective your licence will be deactivated. However, the licence server will not know of your deactivation until you paste the request into online deactivation page.

    This means that it is possible to deactivate a licence but forget to tell the licence server about it, which means you will not be able to reactivate it elsewhere.

    If this happens, reach out to support@ragdolldynamics.com and we may manually deactivate it for you.

https://user-images.githubusercontent.com/2152766/151695127-ad177ba8-f7e7-425a-99df-6aa541c81b8e.mp4 controls
