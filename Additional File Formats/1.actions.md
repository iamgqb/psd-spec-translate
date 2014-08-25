### 附加文件格式 GitHubFlavoredMarkdownOffline
除了用户在Adobe Photoshop中创建的文档外(参看[The Photoshop File Format](http://# "The Photoshop File Format")),有大量附加文件被Photoshop用来储存信息，比如：colors，contours，曲线，色阶等等。这些都是已知的*加载文件*。

这一章描述了每一种加载文件的格式。其中的一些文件可以被用户保存；另外一些则只是加载，as indicated in the sections.

每类文件都有一个唯一的文件类型，并且与它的扩展名关联。Mac 版的Photoshop都可以识别，但它并不要求扩展名。在文件对话框中，Windows 版的Photoshop会自动通过扩展名寻找文件；这是可以被重载的。

在Mac OS下，加载文件的所有信息都存放在data forks中。这些文件在Windows和其他平台中都是可以互换使用的。

考虑到加载文件的读写，字节顺序必须是夸平台的。Photoshop优先在高位中存储多字节，(大端)，如在Mac OS中，这与Windows的标准字节顺序是相反的。为了获得更多的信息，请参看Photoshop API Guide.pdf中的第二章，"Macintosh and Windows development"。

#### 动作
动作可以通过动作面板操作。该对象的效果通过动作机制来输出信息到PSD文件格式中。

<table>
	<caption>Action file types</caption>
<tbody>
	<tr>
		<th>OS</th>
		<th>Filetype/extension</th>
	</tr>
	<tr>
		<td>Mac OS</td>
		<td>8BAC</td>
	</tr>
	<tr>
		<td>Windows</td>
		<td>.ATN</td>
	</tr>
</tbody>
</table>

每一个动作文件包含一个*action set*。下表是动作文件格式的描述。

<table>
	<caption>Action file format</caption>
<tbody>
	<tr>
		<th>Length</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>4</td>
		<td>版本(=16)</td>
	</tr>
	<tr>
		<td>变量</td>
		<td>[Unicode string]:action set的名称</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果set已经被动作面板展开则为true</td>
	</tr>
	<tr>
		<td>4</td>
		<td>action的数量</td>
	</tr>
	<tr>
		<td colspan="2">以下是对每个action的重复</td>
	</tr>
	<tr>
		<td>2</td>
		<td>动作的索引</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果快捷键需要按下shift键则为true</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果快捷键需要按下Command键则为true</td>
	</tr>
	<tr>
		<td>2</td>
		<td>颜色索引信息</td>
	</tr>

	<tr>
		<td>变量</td>
		<td>[Unicode string]:动作名称</td>
	</tr>
	<td>1</td>
		<td>Boolean:如果动作已经被动作面板展开则为true</td>
	</tr>
	<tr>
		<td>4</td>
		<td>步骤的数量在一个action中</td>
	</tr>
	<tr>
	   <td colspan="2">以下是对每一个步骤进行重复</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果动作已经被动作面板展开则为true</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果动作已经被开启</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Boolean:如果应显示对话框则为true</td>
	</tr>
	<tr>
		<td>1</td>
		<td>用于显示对话框的选项</td>
	</tr>
	<tr>
		<td>4</td>
		<td>标识符:'TEXT'或'long'</td>
	</tr>
	<tr>
		<td>变量</td>
		<td>事件:如果标识符是'TEXT',则先是4字节长度，然后是该长度的字符串；如果标识符是'long',则是4字节的itemID</td>
	</tr>
	<tr>
		<td>变量</td>
		<td>字典名称:先是4字节的长度，然后是该长度的字符串</td>
	</tr>
	<tr>
		<td>4</td>
		<td>如果接下来有descriptor，则为-1，没有则为0</td>
	</tr>
	<tr>
		<td>变量</td>
		<td>Descriptor:细节请参看[Descriptor structure]</td>
	</tr>
</tbody>
</table>

<table>
	<caption>Descriptor structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>Unicode string:classID的名称</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>classID:4字节(长度),接下来是字符串或(如果长度是0)4字节的classID</td>
		</tr>
		<tr>
			<td>4</td>
			<td>descriptor中条目</td>
		</tr>
		<tr>
			<td colspan="2">以下是对每一个在descriptor中条目的重复</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>key:4字节(长度),接着是字符串或(如果长度是0)4字节的key</td>
		</tr>
		<tr>
			<td>4</td>
			<td>
				<p>Type:OSType key</p>
				<p>'obj ' = Reference</p>
				<p>'Objc' = Descriptor</p>
				<p>'VlLs' = List</p>
				<p>'doub' = Double</p>
				<p>'UntF' = Unit float</p>
				<p>'TEXT' = String</p>
				<p>'enum'= Enumerated</p>
				<p>'long' = Integer</p>
				<p>'bool' = Boolean</p>
				<p>'GlbO' = GlobalObject same as Descriptor</p>
				<p>'type' or GlbC'= Class</p>
				<p>'alis' = Alias</p>
				<p>'tdta' = Raw Data</p>
			</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>Item type:每种可能的类型请在下表中对照</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Reference Structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>4</td>
			<td>项目的数量</td>
		</tr>
		<tr>
			<td colspan="2">以下是对每一个在reference中条目的重复</td>
		</tr>
		<tr>
			<td>4</td>
			<td>
				<p>OSType key</p>
				<p>'prop' = Property</p>
				<p>'Clss' = Class</p>
				<p>'Enmr' = Enumerated Reference</p>
				<p>'rele' = Offset</p>
				<p>'Idnt' = Identifier</p>
				<p>'indx' = Index</p>
				<p>'name' = Name</p>
			</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>Item type:每种可能的Reference type请在下表中对照</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Property Structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>[Unicode string]:classID的名称</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>classID:4字节(长度)，接着是字符串或(如果长度是0)4字节的classID</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>KeyID:4字节(长度)，接着是字符串或(如果长度是0)4字节的keyID</td>
		</tr>
	</tbody>
</table>

'#Pnt' = points: 带标记的 unit value

'#Mlm' = millimeters: 带标记的 unit value

<table>
	<caption>Unit float structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>4</td>
			<td>
				<p>Units 的值:</p>
				<p>'#Ang' = angle: base degrees</p>
				<p>'#Rsl' = density: base per inch</p>
				<p>'#Rlt' = distance: base 72ppi</p>
				<p>'#Nne' = none: coerced.</p>
				<p>'#Prc'= percent: unit value</p>
				<p>'#Pxl' = pixels: tagged unit value</p>
			</td>
		</tr>
		<tr>
			<td>8</td>
			<td>实际值(double)</td>
		</tr>		
	</tbody>
</table>

<table>
	<caption>Double structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>8</td>
			<td>实际值(double)</td>
		</tr>		
	</tbody>
</table>

<table>
	<caption>Class structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>[Unicode string]:classID的名称</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>ClassID: 4字节(长度)，接着是字符串或(如果长度是0)4字节的classID</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>String structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>[Unicode string]</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Enumerated reference</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>[Unicode string]: ClassID的名称</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>ClassID: 4字节(长度)，接着是字符串或(如果长度是0)4字节的classID</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>TypeID: 4字节(长度)，接着是字符串或(如果长度是0)4字节的TypeID</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>enum: 4字节(长度)，接着是字符串或(如果长度是0)4字节的enum</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Offset structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>[Unicode string]: ClassID的名称</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>ClassID: 4字节(长度)，接着是字符串或(如果长度是0)4字节的classID</td>
		</tr>
		<tr>
			<td>4</td>
			<td>offset的值</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Boolean structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>1</td>
			<td>布尔值</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Alias structure</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>4</td>
			<td>接下来数据的长度</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>Macintosh中的FSSpec或者Windows中完整路径的字符串句柄</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>List structure</caption>
	<tbody>
		<tr>
			<th>length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>4</td>
			<td>list 中项目的数量</td>
		</tr>
		<tr>
			<td colspan="2">以下是对每一个在list中条目的重复</td>
		</tr>
		<tr>
			<td>4</td>
			<td>要使用的类型的OSType key.参看[Descriptor structure]</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>每种可能的类型请参看上面的表</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Integer</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>4</td>
			<td>Value</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Enumerated descriptor</caption>
	<tbody>
		<tr>
			<td>Length</td>
			<td>Description</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>Type: 4字节(长度)，接着是字符串或(如果长度是0)4字节的typeID</td>
		</tr>
		<tr>
			<td>变量</td>
			<td>Enum:4字节(长度)，接着是字符串或(如果长度是0)4字节的enum</td>
		</tr>
	</tbody>
</table>

<table>
	<caption>Raw Data</caption>
	<tbody>
		<tr>
			<th>Length</th>
			<th>Description</th>
		</tr>
		<tr>
			<td>变量</td>
			<td>Value</td>
		</tr>
	</tbody>
</table>