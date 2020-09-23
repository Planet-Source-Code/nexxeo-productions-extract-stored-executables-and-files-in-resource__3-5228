<div align="center">

## Extract stored executables and files in resource


</div>

### Description

Extracts executables or other files stored as binary data in the resource of your executable
 
### More Info
 
resourcetype would be whichever type you put the binary data under, like binary, jpg, text, myfile, etc. Whatever you named it :).

It will also create a directory to put the file in if you want. If not, it will put it in the current directory with the exe.

Example call

WriteFileFromResource(LINK, "BINARY", "link.exe", FALSE, NULL)


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Nexxeo Productions](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/nexxeo-productions.md)
**Level**          |Intermediate
**User Rating**    |4.5 (27 globes from 6 users)
**Compatibility**  |C, C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+
**Category**       |[Files](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/files__3-2.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/nexxeo-productions-extract-stored-executables-and-files-in-resource__3-5228/archive/master.zip)

### API Declarations

```
windows.h
stdio.h
```


### Source Code

```
BOOL WriteFileFromResource(int resourceid, LPCTSTR resourcetype, char* filename, bool createdirectory, char* directory)
{
		if (createdirectory)
		{
			HRESULT result;
			result = CreateDirectory(directory, NULL);
			if (result == 0)
			{
				return FALSE;
			}
			result = SetCurrentDirectory(directory);
			if (result == 0)
			{
				return FALSE;
			}
		}
		HRSRC hRsrc;
		hRsrc = FindResource(NULL, MAKEINTRESOURCE(resourceid), resourcetype);
		if (hRsrc == NULL)
		{
			return FALSE;
		}
		HGLOBAL filedata;
		filedata = LoadResource(NULL, hRsrc);
		if (filedata == NULL)
		{
			return FALSE;
		}
		DWORD size;
		size = SizeofResource(NULL, hRsrc);
		if (size < 0)
		{
			return FALSE;
		}
		FILE* file;
		file = fopen(filename, "wb");
		if (file == NULL)
		{
			return FALSE;
		}
		fwrite(filedata, size, 1, file);
		fclose(file);
		return TRUE;
}
```

