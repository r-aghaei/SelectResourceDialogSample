# Show Property Editor
Sometimes in Windows Forms designer you want to create a werb which shows the property editor for a property (the same property editor which shows in property grid).
You can show the property editor of all properties at design time using code. 
To do so, you can find the internal `EditorServiceContext` which is in `System.Design` assembly, and then invoke its `EditValue` method by passing the your control designer, the control to edit and the property name to edit.

    var editorServiceContext = typeof(ControlDesigner).Assembly.GetTypes()
        .Where(x => x.Name == "EditorServiceContext").FirstOrDefault();
    var editValue = editorServiceContext.GetMethod("EditValue",
        System.Reflection.BindingFlags.Static |
        System.Reflection.BindingFlags.Public);
    editValue.Invoke(null, new object[] { this, this.Component, "MyProperty" });
    
In the example, I've created a user control called `MyControl` which has a `MyControlDesigner` as its designer. 
The designer has a verb which you can see at design time. If you drop an instance of `MyControl` on form, then using the designer action pane or right click on control, you can see `Edit MyProperty` command.
By clicking on the command, the editor for `MyProperty` will be shown.
