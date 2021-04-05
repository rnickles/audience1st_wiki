When working on a multipart feature and you want to temporarily condition out some code (e.g. UI changes related to a feature that isn't complete), you can wrap it with a feature flag.  The `options` table has an attribute `feature_flags` consisting of a serialized array of strings naming features that are enabled.  Remember that this table is maintained per-venue, so a feature can be enabled for some venues but not others.

Pick a name for the feature, say `my_new_feature`.

During development, anywhere that you want to condition code execution on whether the feature is active, do:

```ruby
if Option.feature_enabled?('my_new_feature')
  # do stuff when feature is on
else
  # do stuff when feature is off
end
```

When the code is deployed (in dev or production), the easiest way to turn a feature on or off instantaneously for a venue is to use the Rails console to do so interactively:

```ruby
Apartment::Tenant.switch! 'name_of_tenant'
Option.feature_enabled?('my_new_feature')
# => false
Option.enable_feature! 'my_new_feature'
# feature is now instantaneously enabled
Option.feature_enabled?('my_new_feature')
# => true
Option.disable_feature! 'my_new_feature'
# now it's off again
```
