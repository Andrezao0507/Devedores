# Devedores
 Devedores
unit Unit1;

interface

uses
  Winapi.Windows, Winapi.Messages, System.SysUtils, System.Variants, System.Classes, Vcl.Graphics,
  Vcl.Controls, Vcl.Forms, Vcl.Dialogs, Vcl.StdCtrls, Vcl.ComCtrls, Vcl.ExtCtrls,
  Data.DB, Vcl.Grids, Vcl.DBGrids, FireDAC.Stan.ExprFuncs,
  FireDAC.Phys.SQLiteWrapper.Stat, FireDAC.Phys.SQLiteDef, FireDAC.Stan.Intf,
  FireDAC.Stan.Option, FireDAC.Stan.Param, FireDAC.Stan.Error, FireDAC.DatS,
  FireDAC.Phys.Intf, FireDAC.DApt.Intf, FireDAC.Stan.Async, FireDAC.DApt,
  FireDAC.Comp.DataSet, FireDAC.Comp.Client, FireDAC.Phys, FireDAC.Phys.SQLite,
  FireDAC.UI.Intf, FireDAC.Stan.Def, FireDAC.Stan.Pool, FireDAC.VCLUI.Wait,
  Vcl.Mask, Vcl.DBCtrls;

type
  TTFrmDevedores = class(TForm)
    pnlConteiner: TPanel;
    pcPrincipal: TPageControl;
    Tsconsulta: TTabSheet;
    Tscadastro: TTabSheet;
    pnltop: TPanel;
    btnMarcarPago: TButton;
    btnIncluir: TButton;
    btnAlterar: TButton;
    btnexcluir: TButton;
    pnlGrid: TPanel;
    DBGrid1: TDBGrid;
    FDPhysSQLiteDriverLink1: TFDPhysSQLiteDriverLink;
    dsDevedor: TDataSource;
    qrdevedores: TFDQuery;
    Panel1: TPanel;
    btnSalvar: TButton;
    btnCancelar: TButton;
    conexao: TFDConnection;
    qrdevedorescodigo: TIntegerField;
    qrdevedorespessoa: TStringField;
    qrdevedoresvalor: TBCDField;
    qrdevedoressituacao: TStringField;
    Label1: TLabel;
    edtCodigo: TDBEdit;
    Label2: TLabel;
    edtPessoa: TDBEdit;
    Label3: TLabel;
    edtValor: TDBEdit;
    Label4: TLabel;
    edtSituacao: TDBEdit;
    procedure FormShow(Sender: TObject);
    procedure btnIncluirClick(Sender: TObject);
    procedure btnAlterarClick(Sender: TObject);
    procedure btnSalvarClick(Sender: TObject);
    procedure btnexcluirClick(Sender: TObject);
    procedure btnMarcarPagoClick(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  TFrmDevedores: TTFrmDevedores;

implementation

{$R *.dfm}

procedure TTFrmDevedores.btnAlterarClick(Sender: TObject);
begin
  //editar o registro selcionado da tela de consulta
  qrdevedores.edit;

  //força a abertura da página do cadastro
  pcPrincipal.ActivePage := tsCadastro;
end;

procedure TTFrmDevedores.btnexcluirClick(Sender: TObject);
begin
  qrdevedores.Delete;
end;

procedure TTFrmDevedores.btnIncluirClick(Sender: TObject);
begin
  //inicia a inserção de um novo regsitro na tabela de "devedor"
  qrdevedores.Insert;

  //preenche a coluna "situação" via programação
  qrdevedoressituacao.AsString := 'DEVENDO';

  //força a abertura da página de cadastro
  pcPrincipal.ActivePage := tsCadastro
end;

procedure TTFrmDevedores.btnMarcarPagoClick(Sender: TObject);
begin
  qrdevedores.Edit;

  //alterar o valor da "situaçao" do registro selecionado
  qrdevedoressituacao.AsString := 'PAGO';

  //salta aletraçao feita no registro
  qrdevedores.Post;
end;

procedure TTFrmDevedores.btnSalvarClick(Sender: TObject);
begin
  //salva o registro que esá sendo manipulado
  qrdevedores.Post;

  //força a abertura da página de listagem
  pcPrincipal.ActivePage := tsConsulta;
end;

procedure TTFrmDevedores.FormShow(Sender: TObject);
begin
  //ativa o componente de conexão com o banco de dados
  conexao.Connected := true;
  {ativar conexão da tabela devedor com o componente query}
  qrdevedores.Active := true
end;

end.
