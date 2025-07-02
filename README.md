# ejemplo-2-frm

  import Modelo.Estudiante;
  import javax.swing.*;
  import java.awt.*;
  import java.awt.event.*;
  import java.util.ArrayList;
  
  public class FrmEstudiante extends JFrame {
      private JTextField txtNombre, txtEdad, txtDni, txtCodigo, txtCarrera;
      private JTextArea txtReporte;
      private JButton btnAgregar, btnModificar, btnEliminar, btnListar;
      private JList<String> lstEstudiantes;
      private JComboBox<String> cboEstudiantes;
      private DefaultListModel<String> modeloLista;
      private ArrayList<Estudiante> estudiantes;
      private int indiceSeleccionado = -1;

    public FrmEstudiante() {
        setTitle("Gestión de Estudiantes");
        setSize(600, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        estudiantes = new ArrayList<>();

        JPanel pnlSuperior = new JPanel(new GridLayout(5, 2));
        txtNombre = new JTextField();
        txtEdad = new JTextField();
        txtDni = new JTextField();
        txtCodigo = new JTextField();
        txtCarrera = new JTextField();

        pnlSuperior.add(new JLabel("Nombre:")); pnlSuperior.add(txtNombre);
        pnlSuperior.add(new JLabel("Edad:")); pnlSuperior.add(txtEdad);
        pnlSuperior.add(new JLabel("DNI:")); pnlSuperior.add(txtDni);
        pnlSuperior.add(new JLabel("Código:")); pnlSuperior.add(txtCodigo);
        pnlSuperior.add(new JLabel("Carrera:")); pnlSuperior.add(txtCarrera);

        add(pnlSuperior, BorderLayout.NORTH);

        JPanel pnlCentral = new JPanel();
        modeloLista = new DefaultListModel<>();
        lstEstudiantes = new JList<>(modeloLista);
        //lstEstudiantes.setModel(modeloLista);
        cboEstudiantes = new JComboBox<>();
        txtReporte = new JTextArea(5, 40);
        txtReporte.setEditable(false);
        JScrollPane scrollReporte = new JScrollPane(txtReporte);

        pnlCentral.add(new JLabel("Estudiantes:"));
        pnlCentral.add(lstEstudiantes);
        pnlCentral.add(new JLabel("ComboBox:"));
        pnlCentral.add(cboEstudiantes);
        pnlCentral.add(scrollReporte);

        add(pnlCentral, BorderLayout.CENTER);

        JPanel pnlInferior = new JPanel();
        btnAgregar = new JButton("Agregar");
        btnModificar = new JButton("Modificar");
        btnEliminar = new JButton("Eliminar");
        btnListar = new JButton("Listar");

        pnlInferior.add(btnAgregar);
        pnlInferior.add(btnModificar);
        pnlInferior.add(btnEliminar);
        pnlInferior.add(btnListar);

        add(pnlInferior, BorderLayout.SOUTH);

        // Eventos
        btnAgregar.addActionListener(e -> agregar());
        btnModificar.addActionListener(e -> modificar());
        btnEliminar.addActionListener(e -> eliminar());
        btnListar.addActionListener(e -> listar());

        lstEstudiantes.addListSelectionListener(e -> seleccionarDesdeLista());
        cboEstudiantes.addActionListener(e -> seleccionarDesdeCombo());
    }

    private void agregar() {
        Estudiante est = new Estudiante(
                txtNombre.getText(),
                Integer.parseInt(txtEdad.getText()),
                txtDni.getText(),
                txtCodigo.getText(),
                txtCarrera.getText()
        );
        estudiantes.add(est);
        actualizarVista();
    }

    private void modificar() {
        if (indiceSeleccionado >= 0) {
            //instanciamos una entidad del tipo estudiante
            //le asignamos el indice de la 
            Estudiante est = estudiantes.get(indiceSeleccionado);
            est.setNombre(txtNombre.getText());
            //est.setEdad(Integer.parseInt(txtEdad.getText()));
            est.setDni(txtDni.getText());
            est.setCodigo(txtCodigo.getText());
            est.setCarrera(txtCarrera.getText());
            actualizarVista();
        }
    }

    private void eliminar() {
        if (indiceSeleccionado >= 0) {
            estudiantes.remove(indiceSeleccionado);
            actualizarVista();
        }
    }

    private void listar() {
        //actualiza el contenido de la casilla reporte
        //donde se visualiza todos las instancias de la coleccion: estudiantes
        txtReporte.setText("");
        for (Estudiante e : estudiantes) {
            txtReporte.append(e.resumen() + "\n");
        }
    }

    private void seleccionarDesdeLista() {
        indiceSeleccionado = lstEstudiantes.getSelectedIndex();
        mostrarSeleccionado();
    }

    private void seleccionarDesdeCombo() {
        indiceSeleccionado = cboEstudiantes.getSelectedIndex();
        mostrarSeleccionado();
    }

    private void mostrarSeleccionado() {
        if (indiceSeleccionado >= 0 && indiceSeleccionado < estudiantes.size()) {
            Estudiante est = estudiantes.get(indiceSeleccionado);
            txtNombre.setText( est.getNombre() ) ;
            txtEdad.setText( String.valueOf(est.getEdad()) );
            txtDni.setText( est.getDni() );
            txtCodigo.setText(est.getCodigo());
            txtCarrera.setText(est.getCarrera());
            txtReporte.setText(est.resumen());
        }
    }

    private void actualizarVista() {
        //refrescar el contenido del control lista y combo
        modeloLista.clear();
        cboEstudiantes.removeAllItems();
        for (Estudiante e : estudiantes) {
            modeloLista.addElement(e.getNombre());
            cboEstudiantes.addItem(e.getNombre());
        }
        listar();
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new FrmEstudiante().setVisible(true));
    }
}
