# Epi
Simple snippets/cheatsheet/whatever for EPiServer 7.5

* [Models](#models)
* [ViewModels](#viewmodels)
* [Controllers](#controllers)
* [Views](#views)
* [Stuff](#stuff)
* [Interesting links](#interesting-links)

## Models
### Simple model
#### Example 1
```
using System.ComponentModel.DataAnnotations;

using EPiServer.Core;
using EPiServer.DataAbstraction;
using EPiServer.DataAnnotations;
using DigiOne.Core.DataAbstraction;
using DigiOne.Core.Styles;

using EPiServer;
namespace SomeName.Project.Areas.Layout.Models
{


    [ContentType(DisplayName = "Project block name",
    GroupName = "SomeName Project",
    Description = "Project block.",
    GUID = "E007684C-DE84-4F7D-9BE7-2F786BB4A81F"
    )]
    [ImageUrl("~/modules/SomeName.Project/Client/Icons/page-type-thumbnail.png")]

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
#### Example 2
```
using System.ComponentModel.DataAnnotations;
using EPiServer.DataAbstraction;
using EPiServer.DataAnnotations;

namespace SomeName.Project.Areas.Layout.Models
{
    [ContentType(
        DisplayName = "Project Red Text Block",
        GroupName = "SomeName Project",
        Description = "Text block with red background",
        GUID = "EC2D89E2-CACE-4C87-96E7-CEB2E2B6DFEA"
        )]
    [ImageUrl("~/modules/SomeName.Project/Client/Icons/page-type-thumbnail.png")]
    public class ProjectRedTextData : ProjectBaseBlock
    {
        [CultureSpecific]
        [Display(
            Name = "Text",
            Description = "Text in block",
            GroupName = SystemTabNames.Content,
            Order = 10
            )]
        public virtual string Text { get; set; }
    }
}
```
### Property types
#### Bool
```
[Display(
    Name = "Autoplay",
    Description = "Autoplay",
    GroupName = SystemTabNames.Content,
    Order = 70)]
public virtual bool Autoplay { get; set; }
```
#### String
```
[CultureSpecific]
[Display(
    Name = "Width",
    Description = "Width",
    GroupName = SystemTabNames.Content,
    Order = 50)]
public virtual string Width { get; set; }
```
#### Url
```
[Display(
    Name = "Poster",
    Description = "Url to Poster image",
    GroupName = SystemTabNames.Content,
    Order = 40)]
public virtual Url Poster { get; set; }
```
#### Content area
```
[Display(
    Name = "Main",
    Description = "Main Content",
    GroupName = SystemTabNames.Content,
    Order = 51)]
public virtual ContentArea MainContent { get; set; }
```

#### RTE
```
[CultureSpecific]
[Display(
    Name = "Policy",
    Description = "Policy",
    GroupName = SystemTabNames.Content,
    Order = 65)]
public virtual XhtmlString Policy { get; set; }
```
#### Link Collection(Custom)
```
[Display(
    Name = "Links",
    Description = "Header links",
    GroupName = SystemTabNames.Content,
    Order = 50)]
public virtual LinkItemCollection LinksCollection { get; set; }
```
#### Image reference
NOTE: You can remove UIHint if needed.
```
[UIHint(UIHint.Image)]
[Display(
    Name = "Background image URL",
    Description = "Background image URL",
    GroupName = SystemTabNames.Content,
    Order = 40)]
public virtual ContentReference BackgroundImageUrl { get; set; }
```
#### Block type property
```
[Display(
    Name = "Video content",
    Description = "Drop here VideoPlayer block",
    GroupName = SystemTabNames.Content,
    Order = 100)]
public virtual ProjectVideoPlayerData VideoContent { get; set; }
```
#### Integer
```
[Range(50, 30000)]
[Display(
    Name = "Speed",
    Description = "Autoplay speed",
    GroupName = SystemTabNames.Content,
    Order = 50)]
public virtual Int32 Speed { get; set; }
```
### Property attributes
Required - `[Required]`
Value range - `[Range(50, 2000)]`

### Hide extended property
```
[ScaffoldColumn(false)]
public override String CommonHeadCustomHTML { get; set; }
```

### Allow data types
```
[AllowedTypes(new[] { typeof(UltibroFlameVideoData)})]
```

### Allow page types
```

namespace Boomerang.UsOncology.Website.Areas.BrandPage.Models
{
    [ContentType(
        DisplayName = "BrandPage", 
        GUID = "0e4f7257-ee68-478c-b3d6-f32c342ac46d", 
        Description = "Page used to group Brand Indications page")]
    [AvailableContentTypes(
        Availability.Specific, 
        Include = new[] { 
            typeof(BrandLandingPage.Models.BrandLandingPage),
            typeof(OnePageTemplatePage.Models.OnePageTemplatePage),
            typeof(ServicePage.Models.ServicePage),
            typeof(BlankPage.Models.BlankPage)
        })]
        ......
```

### Default values
```
public override void SetDefaultValues(ContentType contentType)
{
    base.SetDefaultValues(contentType);
    Height = 240;
    Width = 320;
}
```

## ViewModels
### Example 1
```
namespace SomeName.Project.Areas.Layout.ViewModels
{
    public class ProjectRedTextViewModel
    {
        public string Text { get; set; }
    }
}
```
### Example 2
```
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Linq;
using EPiServer.Core;
using DigiOne.Core.Styles;
using EPiServer;

namespace SomeName.Project.Areas.Layout.ViewModels
{


    public class ProjectImageViewModel
    {

        public Url Image { get; set; }

        public String AltText { get; set; }

        public String TitleText { get; set; }

        public Url ImageModal { get; set; }

        public Style Style { get; set; }

    }
}
```
## Views
```
@model SomeName.Project.Areas.Layout.ViewModels.SigniforRedTextViewModel
<div class="RedTextBlock">
    <p>@Model.Text</p>
</div>
```
### Raw string
```
@Html.Raw(@Model.AdditionalHeadCode)
```
### Content Area
```
@(Html.PropertyFor(m => m.Header,
                      new
                          {
                              CustomTag = "header",
                              CssClass = "",
                              ChildrenCustomTagName = "div",
                              ChildrenCssClass = "row",
                          }))
```
### URL for media
```
<div class="video-block" style="background-image: url(@Url.ContentUrl(Model.Image))">
```
### Links
```
@Url.ContentUrl(video.Thumbnail)
```
## Controllers
### Example 1
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web.Mvc;

using SomeName.Project.Areas.Layout.Models;
using SomeName.Project.Areas.Layout.ViewModels;

using EPiServer.Shell;
using EPiServer.Web.Mvc;

namespace SomeName.Project.Areas.Layout.Controllers
{
    public class ProjectImageController : BlockController<ProjectImageData>
    {
        public override ActionResult Index(ProjectImageData currentBlock)
        {
            return PartialView(Paths.ToResource(this.GetType(), "Views/Blocks/Image/Index.cshtml"), new ProjectImageViewModel
            {
                Image = currentBlock.Image,
                AltText = currentBlock.AltText,
                TitleText = currentBlock.TitleText,
                ImageModal = currentBlock.ImageModal,
                Style = currentBlock.Style
            });
        }
    }
}
```
### Example 2
```
using System.Web.Mvc;

using SomeName.Project.Areas.Layout.Models;
using SomeName.Project.Areas.Layout.ViewModels;

using EPiServer.Shell;
using EPiServer.Web.Mvc;

namespace SomeName.Project.Areas.Layout.Controllers
{
    public class ProjectRedTextController : BlockController<ProjectRedTextData>
    {
        public override ActionResult Index(ProjectRedTextData currentContent)
        {
            return this.PartialView(Paths.ToResource(this.GetType(), "Areas/Layout/Views/RedText/Index.cshtml"), new ProjectRedTextViewModel
                                                                              {
                                                                                  Text = currentContent.Text
                                                                              });
        }
    }
}
```
### Adding ulrResolver and contentLoader
```
private readonly IContentLoader _contentLoader;
private readonly UrlResolver _urlResolver;

public UltibroFlameLandingPageController(IContentLoader contentLoader)
{
    _contentLoader = contentLoader;
    _urlResolver = ServiceLocator.Current.GetInstance<UrlResolver>();
}
```
### Getting items from content area
```
var videos = currentPage.VideosArea != null
                ? currentPage.VideosArea.FilteredContents.OfType<UltibroFlameVideoData>().ToList()
                : new List<UltibroFlameVideoData>();
```                
## Stuff
### Media
To make new filetypes available for uploading you need to declare class for each type.
Layout\SomeName.ProjectName\Media\VideoFile.cs

```
using EPiServer.Core;
using EPiServer.DataAnnotations;
using EPiServer.Framework.DataAnnotations;

namespace SomeName.Project.Media
{
    [ContentType]
    [MediaDescriptor(ExtensionString = "flv,mp4,webm,ogv")]
    public class VideoFile : VideoData
    {
        public virtual string AlternativeText { get; set; }
    }
}
```
### Unique IDs
```
@{
    var uniqueId = string.Format("carousel-{0:N}", Guid.NewGuid());
}
```

###  Simple select or list checkboxes property
First of all declare class with data.
```
using System.Collections.Generic;
using System.Linq;
using EPiServer.Shell.ObjectEditing;

namespace Project.Business.Selection
{
    public class ResourceIconSelection : ISelectionFactory
    {
        public IEnumerable<ISelectItem> GetSelections(ExtendedMetadata metadata)
        {
            var iconsList = new SortedDictionary<string, string>();

            iconsList.Add("Link", "link");
            iconsList.Add("Media", "media");
            iconsList.Add("PDF", "pdf");

            return iconsList
                .Select(x => new SelectItem() { Text = x.Key, Value = x.Value })
                .ToArray();
        }
    }
}
```
Next declare property in your model. For select use 'SelectOne' and for list of checkboxes use 'SelectMany'.
```
[SelectOne(SelectionFactoryType = typeof(ResourceIconSelection))]
[Display(
    Name = "Icon",
    GroupName = SystemTabNames.Content,
    Order = 10)]
public virtual string Icon { get; set; }
```

### Get Category
```
var category = article.Category.Any() ? Category.Find(article.Category.First()).LocalizedDescription : string.Empty;
```

### Get children
ex1
```
private readonly IContentLoader _contentLoader;
...
var children =
    FilterForVisitor.Filter(
        _contentLoader.GetChildren<TesArticlePage>(currentPageReference, LanguageSelector.AutoDetect()))
        .OfType<TesArticlePage>()
        .Select(this.toArticleListItem);
```
ex2
```
IEnumerable<ArticlePage> children = 
    DataFactory.Instance.GetChildren(pageLink)
    .Where(child => child is ArticlePage)
    .Cast<ArticlePage>();
```
ex3
```
IEnumerable<TesArticlePage> children = _contentLoader
    .GetChildren<TesArticlePage>(currentPageReference, LanguageSelector.AutoDetect());
```

## Interesting links
* [Awesome EPI](https://github.com/b1thunt3r/awesome-EPiServer)
* [404Handler](https://www.coderesort.com/p/epicode/wiki/404Handler)
* [Using custom CSS in edit mode](http://world.episerver.com/blogs/Ben-McKernan/Dates/2014/12/using-custom-css-in-edit-mode/)
