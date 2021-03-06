UI, a static global class AND an Object class. It is the method to interact with custom UI elements. It allows you to read/write attributes of elements defined in the XML of the UI. It also allows you to receive information from various inputs (like buttons) on-screen and on objects.

!!!attention
    This class allows for the **manipulation** of UI **at runtime**. It does **NOT** modify or fetch **the original XML** in the editor, but rather what is displayed as it continues to run during a game. Just like with Lua, you can only get/set dynamic values during runtime. You can use [onSave](event#onsave) and [onLoad](event#onload) to record any data you want to persist through save/load/undo.
    
    For more information on how to build UI elements within XML, view the [UI API](ui/introUI.md).


##Global and Object
UI can either be placed on the screen by using the **Global UI** or placed on an Object using **Object UI**. Depending on which you are using, these commands are used differently.

    Example of calling a function targeted at the Global UI:
        UI.getAttributes(id)
    Example of calling a function targeted at an Object UI:
        object.UI.getAttributes(id)


##Inputs

[Input Elements](ui/inputelements) are able to trigger a function. By default, Global UI will trigger a function in Global and Object UI will trigger a function in the Object's script.  To change the target script for an input, [view more details here](ui/inputelements#targeting-triggers).

When creating the input element in XML, you will select the name of the function it activates. Regardless of its name, it always will pass parameters

!!!info "functionName(player, value, id)"
    * [<span class="tag pla"></span>](intro#types) **player**: A direct Player reference to the person that triggered the input.
    * [<span class="tag var"></span>](intro#types) **value**: The value sent by the input. A numeric value or a string, generally.
        * {>>This is not used by buttons!<<}
    * [<span class="tag str"></span>](intro#types) **id**: 
        * {>>This is only passed if the element was given an Id attribute in the XML.<<}

```lua
function onButtonClick(player, value, id)
    print(player.steam_name)
    print(id)
end
```

---

##Element Function Summary

Function Name | Description | Return | &nbsp;
-- | -- | -- | --
getAttribute([<span class="tag str"></span>](intro#types)&nbsp;id, [<span class="tag str"></span>](intro#types)&nbsp;attribute) | Obtains the value of a specified attribute of a UI element. | [<span class="ret var"></span>](intro#types) | [<span class="i"></span>](#getattribute)
getAttributes([<span class="tag str"></span>](intro#types)&nbsp;id) | Returns the attributes and their values of a UI element. | [<span class="ret tab"></span>](intro#types) | [<span class="i"></span>](#getattributes)
<a class="anchor" id="getxml"></a>getXml() | Returns the run-time UI's XML in string format. | [<span class="ret str"></span>](intro#types) | 
getXmlTable() | Returns the run-time UI's XML formatted as a Lua table. | [<span class="ret tab"></span>](intro#types) | [<span class="i"></span>](#getxmltable)
hide([<span class="tag str"></span>](intro#types)&nbsp;id) | Hides the given UI element. Unlike the "active" attribute, hide triggers animations. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#hide)
setAttribute([<span class="tag str"></span>](intro#types)&nbsp;id, [<span class="tag str"></span>](intro#types)&nbsp;attribute, [<span class="tag var"></span>](intro#types)&nbsp;value) | Sets the value of a specified attribute of a UI element. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#setattribute)
setXml([<span class="tag str"></span>](intro#types)&nbsp;xml) | Replaces the run-time UI with the XML string. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#setxml)
setXmlTable([<span class="tag tab"></span>](intro#types)&nbsp;data) | Replaces the run-time UI with an XML string which is generated from a table of data. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#setxmltable)
show([<span class="tag str"></span>](intro#types)&nbsp;id) | Displays the given UI element. Unlike the "active" attribute, show triggers animations. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#show)
setAttributes([<span class="tag str"></span>](intro#types)&nbsp;id, [<span class="tag tab"></span>](intro#types)&nbsp;data) | Updates the value of the supplied attributes of a UI element. | [<span class="ret boo"></span>](intro#types) | [<span class="i"></span>](#setattributes)


 
---

##Element Function Details

###getAttribute(...)

[<span class="ret var"></span>](intro#types)&nbsp;Obtains the value of a specified attribute of a UI element. What it returns will typically be a string or a number.

!!!info "getAttribute(id, attribute)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.
    * [<span class="tag str"></span>](intro#types) **attribute**: The name of the attribute you wish to get the value of.
    
``` Lua
self.UI.getAttribute("testElement", "fontSize")
```

---


###getAttributes(...)

[<span class="ret tab"></span>](intro#types)&nbsp;Returns the attributes and their values of a UI element. It only returns the attributes (and values) for elements that have had those attributes set by the user.

!!!info "getAttributes(id)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.

!!!info "Return table"
    * [<span class="tag tab"></span>](intro#types) **parameters**: A Table with the attributes as keys and their XML value as the key's value.
        * [<span class="tag str"></span>](intro#types) **texture**: The name of the image element
        * [<span class="tag str"></span>](intro#types) **color**: The hex used for the color element's value.
    
    **IMPORTANT**: This return table is an example of one you may get back from using it on a RawImage element type. The attribute keys you get back and their values will depend on the element you use the function on as well as the attributes you, the user, have assigned to it.

---


###getXmlTable()

[<span class="ret tab"></span>](intro#types)&nbsp;Obtain the run-time UI formatted as a Lua table of data.

Example Returned Table:
```lua
{
    {
        tag="HorizontalLayout",
        attributes={
            height=200,
            width=1000,
            color="rgba(0,0,0,0.7)",
        },
        children={
            {
                tag="Text",
                attributes={
                    fontSize=100,
                    color="red",
                },
                value="Example",
            },
            {
                tag="Text",
                attributes={
                    text="Message",
                    fontSize=100,
                    color="blue",
                },
            },
        }
    }
}
```

What the XML would look like which returns that table:
```xml
<HorizontalLayout height="200" width="1000" color="rgba(0,0,0,0.7)">
    <Text fontSize="100" color="red">Example</Text>
    <Text text="Message" fontSize="100" color="blue" />
</HorizontalLayout>
```

---


###hide(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Hides the given UI element. Unlike the "active" attribute, hide triggers animations.

!!!info "hide(id)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.

    
``` Lua
self.UI.hide("testElement")
```

---


###setAttribute(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Sets the value of a specified attribute of a UI element.

!!!important
    This will override the run-time value from the XML UI for all players, forcing them to see the same value.

!!!info "setAttribute(id, attribute, value)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.
    * [<span class="tag str"></span>](intro#types) **attribute**: The name of the attribute you want to set the value of.
    * [<span class="tag var"></span>](intro#types) **value**: The value to set for the attribute.

    
``` Lua
self.UI.setAttribute("testElement", "fontSize", 200)
```

---


###setXml(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Replaces the run-time UI with the XML string.

!!!info "setXml(xml)"
    * [<span class="tag str"></span>](intro#types) **xml**: A single string with the contents of the XML to use

    
``` Lua
self.UI.setXml("<Text>Test</Text>")
```

!!!warning
    setXml takes 1 frame to update the runtime UI. This means any change or get of xml/attributes during this frame will not be recognized correctly.

---


###setXmlTable(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Replaces the run-time UI with an XML string which is generated from a table of data.

!!!info "setXmlTable(data)"
    * [<span class="tag tab"></span>](intro#types) **data**: A table containing sub-tables. One sub-table for each element being created.
        * [<span class="tag str"></span>](intro#types) **tag**: The element type. 
        * [<span class="tag tab"></span>](intro#types) **attributes**: A table containing attribute names for keys. Available attribute types depend on tag's element type.
            * {>>Optional, defaults to not being used.<<}
            * {>>Example key/value pairs: text="Test", color="black"<<}
        * [<span class="tag str"></span>](intro#types) **value**: Text that appears `<Text>Here</Text>`, between the `<>` and `</>`.
            * {>>Optional, defaults to an empty string.<<}
        * [<span class="tag tab"></span>](intro#types) **children**: A table containing more sub-tables, formatted as above. This does mean the sub-tables can contain their own children as well, containing sub-sub tables, etc.
            * {>>Optional, defaults to not being used.<<}

```lua
function onLoad()
    UI.setXmlTable({
        {
            tag="HorizontalLayout",
            attributes={
                height=200,
                width=1000,
                color="rgba(0,0,0,0.7)",
            },
            children={
                {
                    tag="Text",
                    attributes={
                        fontSize=100,
                        color="red",
                    },
                    value="Example",
                },
                {
                    tag="Text",
                    attributes={
                        text="Message",
                        fontSize=100,
                        color="blue",
                    },
                },
            }
        }
    })
end
```

!!!warning
    setXmlTable takes 1 frame to update the runtime UI. This means any change or get of xml/attributes during this frame will not be recognized correctly.
    
---


###show(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Shows the given UI element. Unlike the "active" attribute, show triggers animations.

!!!info "show(id)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.

    
``` Lua
self.UI.show("testElement")
```

---


###setAttributes(...)

[<span class="ret boo"></span>](intro#types)&nbsp;Updates the value of the supplied attributes of a UI element. You do not need to set every attribute with the data table, an element will continue using any previous values you do not overwrite.

!!!important
    This will override the run-time value from the XML UI for all players, forcing them to see the same value.

!!!info "setAttributes(id, data)"
    * [<span class="tag str"></span>](intro#types) **id**: The Id that was assigned, as an attribute, to the desired XML UI element.
    * [<span class="tag tab"></span>](intro#types) **data**: A Table with key/value pairs representing attributes and their values.

!!!info "Example data table"
    * [<span class="tag tab"></span>](intro#types) **data**: A Table with parameters which guide the function.
        * [<span class="tag flo"></span>](intro#types) **data.fontSize**: Attribute's desired value value
        * [<span class="tag str"></span>](intro#vector) **data.color**: Attribute's desired value
                    
    **IMPORTANT**: This table is an example of one you may use when setting a text UI element. The attribute keys you use and their values will depend on the element you use the function on.
    
```lua
attributeTable = {
    fontSize = 300,
    color = "#000000"
}
self.UI.setAttributes("exampleText", attributeTable)
```

---
