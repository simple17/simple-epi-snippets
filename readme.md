#Epi
##Models
```
using System.ComponentModel.DataAnnotations;

using EPiServer.Core;
using EPiServer.DataAbstraction;
using EPiServer.DataAnnotations;

namespace Boomerang.Project.Areas.Layout.Models
{
    using DigiOne.Core.DataAbstraction;
    using DigiOne.Core.Styles;

    using EPiServer;

    [ContentType(DisplayName = "Project block name",
    GroupName = "Boomerang Project",
    Description = "Project block.",
    GUID = "E007684C-DE84-4F7D-9BE7-2F786BB4A81F"
    )]
    [ImageUrl("~/modules/Boomerang.Project/Client/Icons/page-type-thumbnail.png")]

    public class ProjectBlockData : BlockData
    {

        [CultureSpecific]
        [Display(
            Name = "Some",
            Description = "Some desc",
            GroupName = SystemTabNames.Content,
            Order = 10)]
        public virtual Url Some { get; set; }
    }
}

```
###Types
####Bool
```
[CultureSpecific]
[Display(
    Name = "Autoplay", 
    Description = "Autoplay", 
    GroupName = SystemTabNames.Content, 
    Order = 70)]
public virtual bool Autoplay { get; set; }
```
####String
```
[CultureSpecific]
[Display(
    Name = "Width",
    Description = "Width",
    GroupName = SystemTabNames.Content,
    Order = 50)]
public virtual string Width { get; set; }
```
####Url
```
[CultureSpecific]
[Display(
    Name = "Poster",
    Description = "Url to Poster image",
    GroupName = SystemTabNames.Content,
    Order = 40)]
public virtual Url Poster { get; set; }
```
###Attributes
Required - `[Required]`

##ViewModels
##Views
##Controllers
##Stuff
###Media
To make a new filetypes available for uploading you need to declare class for each type.
Layout\Boomerang.ProjectName\Media\VideoFile.cs

```
using EPiServer.Core;
using EPiServer.DataAnnotations;
using EPiServer.Framework.DataAnnotations;

namespace Boomerang.Signifor.Media
{
    [ContentType]
    [MediaDescriptor(ExtensionString = "flv,mp4,webm,ogv")]
    public class VideoFile : VideoData
    {
        public virtual string AlternativeText { get; set; }
    }
}
```