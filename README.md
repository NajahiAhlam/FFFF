 this.fournisseurName$ = this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
      map(response => response || 'Unknown'), // Handling null or undefined case
      catchError(() => 'Unknown') // Handling error case
    );
    ------------------------
    <div *ngIf="fournisseurName$ | async as fournisseurName">
  <p>Fournisseur Name: {{ fournisseurName }}</p>
</div>
