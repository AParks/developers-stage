---
layout: plugin-nav-bar
group: intro
subgroup: getting-started
---
# Creating Plugins

Plugins can be created and managed through the Zengine Developer screen, found in the [Developer section of My Account]({{site.clientDomain}}/account/developer){:target="_blank"}. To start, there are two options -- name and namespace. Name will be used to publicly identify your plugin in the app. Namespace is a unique identifier to be used by your plugin code to distinguish it from other plugins. Namespace is not publicly displayed, but it will be used in several places in your plugin code. Name can be changed, but namespace cannot be changed after your plugin is created.

After you have provided a plugin name and namespace, you will be taken into the plugin developer console to edit your newly created plugin. Plugins consist of 3 pieces of data -- CSS, HTML, and JavaScript. The initial code is a sample Hello World plugin, populated with your specific plugin options. Notice that your namespace appears in the plugin HTML template ID and in the JavaScript controller and registration parameters.  You are required to prefix all component names in your plugin with this namespace.

Plugins are written in JavaScript and use [AngularJS]({{site.baseurl}}/plugins/development). If you open the initial plugin HTML, you will see that it contains an AngularJS template with a binding for `{% raw %}{{text}}{% endraw %}`.

{% highlight html %}
{% raw %}
<script type="text/ng-template" id="my-plugin-main">
    <div class="title">
        <h1>{{text}}</h1>
    </div>
</script>
{% endraw %}
{% endhighlight%}

If you click to the plugin JavaScript, you can see where it dynamically sets the `text` property to `Hello World!`

{% highlight javascript %}
{% raw %}
plugin.controller('myPluginCntl', ['$scope', function ($scope) {

    $scope.text = 'Hello World!';

}])
{% endraw %}
{% endhighlight %}

If you click into CSS, you can see the CSS that will apply to your HTML. CSS will be scoped by your namespace when published, so your CSS will only apply inside your plugin.

{% highlight css %}
{% raw %}
/**
 * Plugin My Plugin CSS
 */

.title { 
    color: purple;
}
{% endraw %}
{% endhighlight %}

### Registration Options

There are various plugin options you can change through the registration options. The following options are the only ones you need to get a [full-page plugin]({{site.baseurl}}/plugins/getting-started/plugin-types.html) up and running. 

A plugin can have one or more `interfaces`. This interfaces allows you to develop a plugin that with different types, for example a common use case is a plugin type `full-page` and `settings`.

{% highlight javascript %}
{% raw %}
/**
 * Plugin Registration
 */
.register('myPlugin', {
    route: '/myplugin',
    controller: 'myPluginCntl',
    template: 'my-plugin-main',
    type: 'fullPage'
});

/**
 * Plugin Registration
 */
.register('myPlugin', {
	route: '/myPlugin',
	interfaces: [
		{
			controller: 'myPluginCntl',
			template: 'my-plugin-main',
			type: 'fullPage',
		}
	]
});

{% endraw %}
{% endhighlight %}

A plugin can also have multiple `interfaces`.
This allows you to develop a plugin with different types, for example a common use case is a plugin with both `full-page` and `settings` types.

{% highlight javascript %}
{% raw %}
/**
 * Plugin Registration
 */
.register('myPlugin', {
    route: '/myplugin',
    controller: 'myPluginCntl',
    template: 'my-plugin-main',
    type: 'fullPage'
});

/**
 * Plugin Registration
 */
.register('myPlugin', {
	route: '/myPlugin',
	interfaces: [
		{
			controller: 'myPluginCntl',
			template: 'my-plugin-main',
			type: 'fullPage',
		},
		{
			controller: 'myPluginSettingsCntl',
			template: 'my-plugin-settings',
			type: 'settings'
		}
	]
});

{% endraw %}
{% endhighlight %}


The `route` parameter represents the URI path to run your plugin. If your route is `/myplugin`, then the full URI to your plugin might be `{{ site.clientDomain }}/workspaces/123/plugin/myplugin`

You can have multiple controllers in your plugin JavaScript. The `controller` parameter represents the main controller name. Note that all controller names are prefixed with your namespace, like `myPluginCntl`.

Similar to controller, your plugin HTML can have multiple templates. The `template` parameter corresponds to the template associated with the main controller. This value represents a template ID in the plugin HTML. The template ID must be prefixed with a dash-delimited version of your namespace, like `my-plugin-main`. This is in keeping with the AngularJS HTML attribute style.

# Running Plugins

You can test your plugin as you develop by clicking the **Run** button in the top right corner. When you run your plugin, you will be taken out of the editor and back to the app. Navigate to one of your workspaces and you will notice a new icon for the plugin in the header. Hover over the icon and it should display the plugin name. Click on the icon and it will open the plugin to display the Hello World text.

At this point, your plugin is not published. Only you can access and run your plugin.

To get back to the plugin editor, hover over the Dev Mode menu in the right of the header and click View Editor.

# Publishing Plugins

You can get to the publishing screen by clicking the Publishing Settings button in the plugin editor. From there you can confirm settings such as the name and description. You can also provide a Firebase URL and optionally a Firebase secret. The Firebase secret will allow a plugin to take advantage of Firebase authentication. 

Plugins are available only to the developer prior to publishing. Once a plugin is published, different rules apply, depending on whether it's a private or public plugin:

- If it's a **private** (and therefore workspace-level) plugin, it can be shared with specific workspaces for all users to use in that workspace. After publishing, a list of workspaces where the plugin developer is a member will appear. The plugin developer can choose to add or remove access to the plugin by workspace. The plugin developer can also revoke access entirely by deleteing the plugin through the API.
- If it's a **public** plugin, it will appear on the marketplace for anyone to use. Although it can't be deleted, the plugin developer has the option of hiding it from the marketplace.


More information about publishing is available [here]({{site.baseurl}}/plugins/getting-started/publishing.html).
