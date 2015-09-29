#Epi
##Models
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