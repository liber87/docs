$out='';
//С помощью DBAPI
$id = $modx->event->params['id'];
$tvid = $modx->db->getValue('select id from '.$modx->getFullTableName('site_tmplvars').' where name = "images"');
$images = $_POST['tv'.$tvid];
$out.='Image from DB - '.$tv.'<br>';

$tvid = '26'; //id тв получаемого параметра
$tvname = 'images'; //название получаемого параметра

//Из POST
$out.='Image from POST - '.$_POST['tv'.$tvid].'<br>';

// Из Global
global $tmplvars;
$out.='Image from tmplvars - '.$tmplvars[$tvid][1].'<br>';

// C помощью API через getTemplateVar и getTemplateVarOutput
$tvtv = $modx->getTemplateVar($tvname,'*',$id);
$out.='Image whith out getTemplateVar - '.$tvtv['value'].'<br>';

$tvtvo =$modx->getTemplateVarOutput($tvname,$id);
$out.='Image whith out getTemplateVarOutput - '.$tvtvo[$tvname].'<br>';

//Через установку плейсхолдера
$modx->setPlaceholder('tvsp',$_POST['tv'.$tvid]);
$out.='Image from placeholder - '.$modx->getPlaceholder('tvsp').'<br>';

//С помощью MODxAPI
include_once(MODX_BASE_PATH.'assets/lib/MODxAPI/modResource.php');
$doc = new modResource($modx);
$im = $doc->edit($id)->get($tvname);
$out.='Image from modResourse - '.$im.'<br>';

//Через DocLister
$dl_images = $modx->runSnippet('DocLister',array('documents'=>$id,'tvPrefix'=>'','tvList'=>$tvname,'tpl'=>'@CODE: [+'.$tvname.'+]'));
$out.='Images from DocLister - '.$dl_images;

echo $out;
exit();

![Вывод](https://github.com/mediakot/docs/blob/master/ru/images/gettv.jpg)
