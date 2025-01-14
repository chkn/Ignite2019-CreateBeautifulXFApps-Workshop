# Create Beautiful Xamarin.Forms Apps – Ignite Workshop

## Objectives:

You should leave this workshop today knowing the following:

- how to create beautiful Xamarin.Forms app
- Use Hot Reload to iterate on your UI


## Before we begin – Choose your setup:

What OS are you using?

[Mac](#mac) | [Windows](#android)

### <a id="mac"></a>Mac

Do you have Xcode installed?

[Yes](#macios) | [No](#android)

# <a id="macios"></a>Visual Studio for Mac and iOS

## Get set up

1. Download and open the sample from GitHub in Visual Studio for Mac.

2. Open the .sln file within the **Start** folder. Once it opens in the IDE, double click on `TodoListPage.xaml` in the Views folder to open it in the editor.

3. The next step is to enable the Hot Reload feature, which allows us to preview changes while the app is running. To do this go to: **Visual Studio > Preferences > Project > XAML Hot Reload** and select **Enable Hot Reload (Preview)**.

4. Start debugging the app by selecting **Debug | iOS Simulator** in the toolbar and press the Play button.
Feel free to play around with the app in the simulator.

## Use FontImageSource for beautiful icons

5. We want to update the toolbar button to something that looks a little better. This is done by using the `FontImageSource` capabilities available in Xamarin.Forms 4.0 and later. We have added the font file to the projects for you, and added the font as an app-wide `StaticResource`. To change the toolbar icon, replace the `ToolbarItem.IconImageSource` code in the `TodoListPage.xaml` file with:

    ```xml
    <ToolbarItem.IconImageSource>
        <FontImageSource 
                Glyph="&#xf183;"
                FontFamily="{StaticResource MaterialFontFamily}"
                Size="32" />
    </ToolbarItem.IconImageSource>
    ```

    The glyphs are represented as a 4 character unicode symbol starting with '\', however you have to escape the backslash using `&#x`.

    We've chosen to use the "Add Message" glyph, but you can use any that you like from the [Material Design icons](https://cdn.materialdesignicons.com/4.5.95/) website.

6. Do the same with checkmark icon for each item. Find a [glyph]((https://cdn.materialdesignicons.com/4.5.95/)) you'd like to replace the existing icon with. We can use a markup extension to make the code neater.

    Replace the `<Image>` tag with:

    ```xml
    <Image HorizontalOptions="End" IsVisible="{Binding Done}"
            Source="{FontImage FontFamily={StaticResource MaterialFontFamily},
                                Glyph=&#xf12c;,
                                Color=Green,
                                Size=32}" />
    ```

    Here you are changing the source from a file (string) to a `FontImageSource`, a new feature in Xamarin.Forms 4.0.

## Using a Collection View

7. Now let's swap out the `ListView` for a more performant alternative - `CollectionView`, which is in preview in Xamarin.Forms 4.2. `CollectionView` automatically utilizes the virtualization capabilities of each native platform to make your lists appear faster. It also supports multiple columns of items, and a simpler API with no need for Cells. There are a few steps to change your `ListView` to a `CollectionView`:

    7 a. Stop debugging your app, as we are going to change some C#, which can't be done during a debug session.

    7b. Go into the `TodoListPage.xaml.cs` file and comment out the `OnListItemSelected` function. Uncomment the version of it below labeled step 6.

    7c. Open `TodoListPage.xaml`. Change all instances of `ListView` to `CollectionView`.

    7d. Add `SelectionMode="Single"` to the opening tag of the `CollectionView`. This tells the `CollectionView` that you are only ever selecting one item at a time.

    7e. Replace `ItemSelected` with `SelectionChanged` in the opening tag of the `CollectionView`.

    7f. Delete the opening and closing `ViewCell` tags (keep the Grid and it's contents inside - CollectionView doesn't need a cell!)

    At the end of step 7, the XAML for your `CollectionView` should look like:

    ```xml
    <CollectionView x:Name="myItems"
                    Margin="20"
                    SelectionMode="Single"
                    SelectionChanged="OnListItemSelected">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                        <Grid Padding="10">
                            <!--#region Grid definitions-->
                            <Grid.RowDefinitions>
                                <RowDefinition Height="Auto" />
                                <RowDefinition Height="Auto" />
                            </Grid.RowDefinitions>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="Auto" />
                                <ColumnDefinition Width="Auto" />
                            </Grid.ColumnDefinitions>
                            <!--#endregion-->
                            <Label Grid.Column="1"
                                Text="{Binding Name}"
                                FontAttributes="Bold" />
                            <Image Grid.Column="2" HorizontalOptions="End" Grid.RowSpan="2" IsVisible="{Binding Done}"
                                Source="{FontImage FontFamily={StaticResource MaterialFontFamily},
                                        Glyph=&#xf12c;,
                                        Color=Green,
                                        Size=32}" />
                        </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    Start debugging your app again so we can begin customizing the `CollectionView` using XAML Hot Reload!

## Working with Frames and Layouts

8. We'll add some color to make the items stand out from the background. Under the data template on line XX. Add a frame around the grid with a background color. We also need to set the `IsClippedToBounds=True` property on the Frame, to ensure that it respects the bounds:

    ```xml
    <Frame BackgroundColour="Aquamarine" IsClippedToBounds="True>
    ...
    </Frame>
    ```

9. With the background set, you can notice that everything looks bad and the items are overlapping. To fix this, we'll add some spacing around the items:

    ```xml
    <CollectionView.ItemsLayout>
                <ListItemsLayout ItemSpacing="20" Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ```

10. Next is make a frame around your stack layout so the items are cute - choose your own background color and text color!

    ```xml
    <Frame BackgroundColor="LightPink" Padding="10" IsClippedToBounds="True" CornerRadius="5">
        <Label Grid.Column="1"
                Text="{Binding Name}"
                FontAttributes="Bold" />
        <Image Grid.Column="2" HorizontalOptions="End" Grid.RowSpan="2" IsVisible="{Binding Done}"
                Source="{FontImage FontFamily={StaticResource MaterialFontFamily},
                        Glyph=&#xf12c;,
                        Color=Green,
                        Size=32}" />
        </Image>
    </Frame>
    ```

    Let's add the ability to see the item description on the page too. Add another label with Grid.Row="1" and set Text={Binding Notes}. Customize the text however you like.

## Using Xamarin.Forms Shell

Xamarin.Forms Shell reduces the complexity of mobile app development by providing a single place to describe the visual hierarchy of an application. It also benefits your application by increased rendering speed and reduced memory consumption. For more information, see the [Xamarin.Forms Shell](https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell/introduction) documentation.

11. Next step is to comment out the initialization step of App.xaml.cs, as we are going to use Shell for describe our hierarchy. We have already added the AppShell.xaml file describing a tabbed page for you. 

12. Add (or comment out) initializing a new Shell page in App.xaml.cs. 

13. Start debugging the app again and change around colors in AppShell.xaml to customize your app!

## Using Material Design

Blurb about material https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/visual/material-visual

14. We've already added the **Xamarin.Forms.Visual.Material** NuGet package and initialized it in your iOS and Android settings (How?).

15. On `TodoItemPage.xaml` page add `Visual="Material"` to any of the buttons and hit save. See the material style button updated with hot reload.

16. Remove that or change "Material" to "Default", and then add Visual="Material" to the overall headers of the page and save. Observe how it updates most of the controls on the page and makes it look guuuuuud. There are also nicer effects and drop shadows interacting with stuffs. If you add Material="Default" to any of the controls on the page you can override the page-set visual=material setting for that control.

## Adding Effects

13. Effects

Finally we gon make this  MOOOOOOOOOOOOVe.!

Stop debuggin cuz gotta edit C# so no hot reload rip

Add this XAML to TodoItemPage after the buttons:
```xml
<Image x:Name="doneImage"
               VerticalOptions="EndAndExpand"
               Margin="10"
               Source="{FontImage FontFamily={StaticResource MaterialFontFamily},
                                    Color=Green,
                                    Glyph=&#xf12c;,
                                    Size=64}"
               Opacity="0"/>
```

This is our good friend the checkmark. Let's make it fade in and grow in size when you mark something as done.


We've given you a blank event handler for when the switch is toggled. When it's toggled off, we want to set our image back to 0 opacity and its original size, so you don't see it! When it's toggled on, we want to fade in the checkmark and grow it in size!

Turn
```csharp
async void Switch_ToggledAsync(object sender, ToggledEventArgs e)
        {
            // Part 13: Effects
            if (e.Value == false)
            {
                // fade out
                // scale down to original
            }
            else if (e.Value == true)
            {
                // fade in
                // scale up to 2x
            }
        }

```

to

```csharp
async void Switch_ToggledAsync(object sender, ToggledEventArgs e)
        {
            // Part 13: Effects
            if (e.Value == false)
            {
                await doneImage.FadeTo(0, 1000);
                await doneImage.ScaleTo(1, 500);
            }
            else if (e.Value == true)
            {
                await doneImage.FadeTo(1, 1000);
                await doneImage.ScaleTo(2, 2000);
            }
        }
```



### <a id="android"></a>Android

1. Download and open the sample from GitHub in Visual Studio.

