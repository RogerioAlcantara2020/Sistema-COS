unit Fprodu;

interface

uses
  Windows, Messages, SysUtils, Classes, Graphics, Controls, Forms, Dialogs,
  ComCtrls,  StdCtrls, Buttons, ExtCtrls, Db, DAODataset, DAOQuery, Grids,
  DBGrids, FileCtrl, DBCtrls, Menus;

type
  TFormProducao = class(TForm)
    GroupBox1: TGroupBox;
    Label7: TLabel;
    dataUm: TDateTimePicker;
    Label1: TLabel;
    dataDois: TDateTimePicker;
    DBGrid1: TDBGrid;
    dsProducao: TDataSource;
    tbl_producao: TDAOQuery;
    Bevel1: TBevel;
    Bevel3: TBevel;
    SpeedButton1: TSpeedButton;
    SpeedButton2: TSpeedButton;
    Label2: TLabel;
    Label3: TLabel;
    Label4: TLabel;
    Bevel4: TBevel;
    arquivo: TEdit;
    BitBtn4: TBitBtn;
    Edit1: TEdit;
    DirectoryListBox1: TDirectoryListBox;
    caminho_encotrado: TLabel;
    dimensao: TRadioButton;
    producao: TRadioButton;
    GroupBox2: TGroupBox;
    Label5: TLabel;
    soma: TDAOQuery;
    ComboBox1: TComboBox;
    cod_unico: TLabel;
    PopupMenu1: TPopupMenu;
    Pesquisar1: TMenuItem;
    Fechar1: TMenuItem;
    status21: TLabel;
    status: TComboBox;
    Label6: TLabel;
    os: TEdit;
    rs_os: TRadioButton;
    rd_status: TRadioButton;
    procedure FormClose(Sender: TObject; var Action: TCloseAction);
    procedure SpeedButton1Click(Sender: TObject);
    procedure SpeedButton2Click(Sender: TObject);
    procedure selopicional;
    procedure DBGrid1CellClick(Column: TColumn);
    procedure BitBtn4Click(Sender: TObject);
    procedure ComboBox1Click(Sender: TObject);
    procedure rd_statusClick(Sender: TObject);
    procedure rs_osClick(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  FormProducao: TFormProducao;

implementation

uses Data_m,ShellAPi, FCentral;
{$R *.DFM}

procedure TFormProducao.FormClose(Sender: TObject;
  var Action: TCloseAction);
begin
Release;
end;

procedure TFormProducao.SpeedButton1Click(Sender: TObject);

var valor,vl:currency;
area,inicio,Final:string;

begin
inicio:=DateToSTr(dataUm.date);
Final:=DateToStr(DataDois.date);

if(status.Text='CRIAÇÃO DA  ARTE')then area:='prazo_arte' else area:='producao';

with tbl_producao do
begin
Close;
SQL.Clear;
if os.text=''then begin
SQL.Text := 'SELECT * FROM tbl_os WHERE '+area+' >=:pInicial and '+area+'<=:pFinal and status_producao=:pStatus';
ParamByName('pInicial').AsDateTime := StrToDate(Inicio);
ParamByName('pFinal').AsDateTime := StrToDate(Final);
ParamByName('pStatus').AsString := status.text;
                   end;

if os.text<>''then begin
SQL.Text := 'SELECT * FROM tbl_os WHERE  identificacao=:pStatus';
ParamByName('pStatus').AsString := os.text;
                   end;

Open;
Active:=true;
end;


if dimensao.Checked=true then
with soma do
begin
Close;
SQL.Clear;
SQL.Text := 'SELECT sum(dimensao) as Preco FROM tbl_os WHERE producao >=:pInicial and producao<=:pFinal';
ParamByName('pInicial').AsDateTime := StrToDate(Inicio);
ParamByName('pFinal').AsDateTime := StrToDate(Final);
Open;
Active:=true;

if fieldbyname('Preco').Value<>null then
label5.Caption:= formatfloat('#.00',(fieldbyname('Preco').Value));
end;
cod_unico.caption:='cod_unico';
end;

procedure TFormProducao.SpeedButton2Click(Sender: TObject);
begin
close;
end;

procedure TFormProducao.selopicional();
begin
  with dm.tbl_opicionais do
       begin
       close;
       sql.Clear;
       Sql.Add('Select* From tbl_opicional where id_venda='+quotedstr(tbl_producao.FieldByName('cod_unico').AsString));
       Active:=true;
end;
end;

procedure TFormProducao.DBGrid1CellClick(Column: TColumn);
var F: TextFile;
tamanho,tamanho2:integer;
Dados,str:String;

begin
selopicional;
arquivo.text:=tbl_producao.FieldByName('arquivo').AsString;
cod_unico.caption:=tbl_producao.FieldByName('cod_unico').AsString;

if fileExists('C:\windows\configuration.txt')then
  begin
 AssignFile(F, 'C:\windows\configuration.txt');
 Reset (F);
 ReadLn(F,Dados);
 edit1.text:=Dados;
 DirectoryListBox1.Directory:=edit1.text;

 tamanho:=pos('arquivos',(arquivo.text));
 str:=(Copy(arquivo.text,27,600));

 tamanho2:=pos('COS',(edit1.text));

 caminho_encotrado.caption:=(Copy(edit1.text,0,21)+'arquivos\'+str);








 end;

end;

procedure TFormProducao.BitBtn4Click(Sender: TObject);
var caminho:string;
begin
        caminho:=arquivo.text;
    if (fileExists(caminho)) then
       ShellExecute(Handle, nil, Pchar(caminho), nil, nil, SW_SHOWNORMAL);
end;

procedure TFormProducao.ComboBox1Click(Sender: TObject);
begin

if cod_unico.caption='cod_unico' then
begin
ShowMessage('Selecione algum serviço para alterar o Status');
exit;
end;

if tbl_producao.FieldByName('status_producao').AsString='PRONTO' then
begin
ShowMessage('Status PRONTO não pode ser alterado.');
exit;
end;

//tbl_producao.Open;
tbl_producao.Edit;
tbl_producao.FieldByName('status_producao').AsString:=Combobox1.Text;
tbl_producao.FieldByName('operador').AsString:=fcontrole.operador.caption;
tbl_producao.Post;

//SpeedButton1.Click;

tbl_producao.Locate('cod_unico',cod_unico.Caption,[]);
end;


procedure TFormProducao.rd_statusClick(Sender: TObject);
begin
os.Clear;
os.ReadOnly:=true;

status.Enabled:=true

end;

procedure TFormProducao.rs_osClick(Sender: TObject);
begin

os.ReadOnly:=false;
os.SetFocus;

status.Enabled:=false;
status.Text:='';
end;

end.
